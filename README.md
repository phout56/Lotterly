// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Lottery {
    address public owner;
    address[] public players;
    uint256 public ticketPrice = 0.002 ether;

    constructor() {
        owner = msg.sender;
    }

    function buyTicket() public payable {
        require(msg.value == ticketPrice, "Incorrect ticket price");
        players.push(msg.sender);
    }

    function getPlayers() public view returns (address[] memory) {
        return players;
    }

    function drawWinner() public onlyOwner {
        require(players.length > 0, "No players");

        uint256 winnerIndex = random() % players.length;
        address winner = players[winnerIndex];

        payable(winner).transfer(address(this).balance);

        delete players;
    }

   function random() private view returns (uint256) {
    return uint256(
        keccak256(abi.encodePacked(block.timestamp, block.basefee))
    );
}

    modifier onlyOwner() {
        require(msg.sender == owner, "Only owner");
        _;
    }
}

