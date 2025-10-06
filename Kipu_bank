//SPDX-License-Identifier: MIT
pragma solidity ^0.8.30;


/// @title KipuBank - A secure decentralized ETH vault
/// @author NatalyAleksi
/// @notice This contract allows users to deposit and withdraw ETH with individual and global limits.
/// @dev Built for the Kipu Web3 Developer Exam

contract KipuBank {
    /*//////////////////////////////////////////////////////////////
                                ERRORS
    //////////////////////////////////////////////////////////////*/

    /// @dev Thrown when a user tries to withdraw more than allowed
    error WithdrawalLimitExceeded(uint256 requested, uint256 limit);

    /// @dev Thrown when the bank cap is reached
    error BankCapReached(uint256 current, uint256 cap);

    /// @dev Thrown when the user tries to withdraw without balance
    error InsufficientBalance();

    /// @dev Thrown if the transfer fails
    error TransferFailed();

    /*//////////////////////////////////////////////////////////////
                            STATE VARIABLES
    //////////////////////////////////////////////////////////////*/

    /// @notice Immutable maximum deposit cap for the entire bank
    uint256 public immutable bankCap;

    /// @notice Maximum amount a user can withdraw per transaction
    uint256 public constant WITHDRAW_LIMIT = 0.5 ether;

    /// @notice Total amount of ETH deposited in the bank
    uint256 public totalDeposits;

    /// @notice Tracks user balances
    mapping(address => uint256) private balances;

    /*//////////////////////////////////////////////////////////////
                                EVENTS
    //////////////////////////////////////////////////////////////*/

    /// @notice Emitted when a user makes a successful deposit
    /// @param user Address of the depositor
    /// @param amount Amount of ETH deposited
    event Deposit(address indexed user, uint256 amount);

    /// @notice Emitted when a user withdraws ETH
    /// @param user Address of the withdrawing user
    /// @param amount Amount of ETH withdrawn
    event Withdrawal(address indexed user, uint256 amount);

    /*//////////////////////////////////////////////////////////////
                                CONSTRUCTOR
    //////////////////////////////////////////////////////////////*/

    /// @param _bankCap Sets the total ETH limit allowed for all deposits
    constructor(uint256 _bankCap) payable {
        bankCap = _bankCap;
    }

    /*//////////////////////////////////////////////////////////////
                                MODIFIERS
    //////////////////////////////////////////////////////////////*/

    /// @dev Ensures that total deposits don't exceed the bank cap
    modifier withinBankCap(uint256 _amount) {
        if (totalDeposits + _amount > bankCap) {
            revert BankCapReached(totalDeposits, bankCap);
        }
        _;
    }

    /*//////////////////////////////////////////////////////////////
                            EXTERNAL FUNCTIONS
    //////////////////////////////////////////////////////////////*/

    /// @notice Deposit ETH into your personal vault
    /// @dev Follows Checks–Effects–Interactions pattern
    /// @custom:error BankCapReached if the global cap is exceeded
    /// @custom:event Deposit
    function deposit() external payable withinBankCap(msg.value) {
        if (msg.value == 0) revert("Deposit must be > 0");

        // Effects
        balances[msg.sender] += msg.value;
        totalDeposits += msg.value;

        // Interaction (none external)
        emit Deposit(msg.sender, msg.value);
    }

    /// @notice Withdraw ETH from your vault up to the allowed limit
    /// @param _amount Amount of ETH to withdraw
    /// @custom:error WithdrawalLimitExceeded if exceeds limit
    /// @custom:error InsufficientBalance if user balance < amount
    /// @custom:event Withdrawal
    function withdraw(uint256 _amount) external {
        uint256 userBalance = balances[msg.sender];

        if (_amount > WITHDRAW_LIMIT)
            revert WithdrawalLimitExceeded(_amount, WITHDRAW_LIMIT);
        if (_amount == 0 || userBalance < _amount)
            revert InsufficientBalance();

        // Effects
        balances[msg.sender] -= _amount;
        totalDeposits -= _amount;

        // Interaction
        _transferFunds(msg.sender, _amount);

        emit Withdrawal(msg.sender, _amount);
    }

    /*//////////////////////////////////////////////////////////////
                            PRIVATE FUNCTIONS
    //////////////////////////////////////////////////////////////*/

    /// @dev Internal function to safely transfer ETH
    /// @param _to Receiver address
    /// @param _amount Amount of ETH to send
    function _transferFunds(address _to, uint256 _amount) private {
        (bool success, ) = _to.call{value: _amount}("");
        if (!success) revert TransferFailed();
    }

    /*//////////////////////////////////////////////////////////////
                                VIEW FUNCTIONS
    //////////////////////////////////////////////////////////////*/

    /// @notice Get your current vault balance
    /// @return The user's ETH balance in the contract
    function getBalance() external view returns (uint256) {
        return balances[msg.sender];
    }
}
