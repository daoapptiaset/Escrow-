# Escrow-
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract SimpleEscrow {
    address public buyer;
    address payable public seller;
    uint256 public amount;
    bool public released;

    constructor(address payable _seller, uint256 _amount) payable {
        buyer = msg.sender;
        seller = _seller;
        amount = _amount;
        require(msg.value == amount, "Send exact amount");
    }

    function release() external {
        require(msg.sender == buyer, "Only buyer");
        require(!released, "Already released");
        released = true;
        seller.transfer(amount);
    }

    function refund() external {
        require(msg.sender == seller, "Only seller");
        require(!released, "Already released");
        payable(buyer).transfer(amount);
    }
}
