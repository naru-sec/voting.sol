// SPDX-License-Identifier: MIT
pragma solidity >=0.6.12 <0.9.0;

contract voting {

    address public owner;
    mapping(address => bool) public hasVoted;
    mapping(address => uint) public votes;
    bool public votingOpen = false;
    
    // List to store candidate names
    string[] public candidates;

    constructor() {
        owner = msg.sender;
    }

    // 1. Function to add candidates (Only owner can add)
    function addCandidate(string memory name) public {
        require(msg.sender == owner, "Only the owner can add candidates!");
        candidates.push(name);
    }

    // 2. Function to toggle voting on or off (Only owner can control)
    function toggleVoting() public {
        require(msg.sender == owner, "Only the owner can toggle voting!");
        votingOpen = !votingOpen; 
    }

    // 3. Function to cast a vote
    function vote(address candidate) public {
        require(votingOpen, "Voting is not open right now!");
        require(!hasVoted[msg.sender], "You have already voted!");

        hasVoted[msg.sender] = true; 
        votes[candidate] += 1;       
    }

    // 4. Function to check the total number of candidates
    function getCandidatesCount() public view returns (uint) {
        return candidates.length;
    }
}
