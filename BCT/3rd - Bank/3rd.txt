// SPDX-License-Identifier: UNLICENSED
pragma solidity 0.8.30;

contract Bank {
    uint256 private balance;
    address public accOwner;

    constructor() payable {
        accOwner = msg.sender;
        balance = msg.value;
    }

    function deposit() public payable {
        balance += msg.value;
    }

    function withdraw(uint256 amount) public {
        require(accOwner == msg.sender, "You are not the account owner");
        require(amount > 0, "Withdrawal amount should be greater than 0");
        require(amount <= balance, "Insufficient balance");

        balance -= amount;
        payable(msg.sender).transfer(amount);
    }

    function showBalance() public view returns (uint256) {
        require(accOwner == msg.sender, "You are not the account owner");
        return balance;
    }

    receive() external payable {
        balance += msg.value;
    }

    fallback() external payable {
        balance += msg.value;
    }
}
