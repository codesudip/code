// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract VotingSystem {
    // Structure to represent a candidate
    struct Candidate {
        uint id;
        string name;
        uint voteCount;
    }

    // Mapping from candidate ID to Candidate
    mapping(uint => Candidate) public candidates;

    // Mapping from address to voted status
    mapping(address => bool) public voters;

    // Number of candidates
    uint public candidatesCount;

    // Number of votes
    uint public totalVotes;

    // Event that is emitted when a vote is cast
    event Voted(address indexed voter, uint candidateId);

    constructor() {
        // Add candidates
        addCandidate("Alice");
        addCandidate("Bob");
    }

    // Function to add a candidate
    function addCandidate(string memory _name) private {
        candidatesCount++;
        candidates[candidatesCount] = Candidate(candidatesCount, _name, 0);
    }

    // Function to vote for a candidate
    function vote(uint _candidateId) public {
        require(!voters[msg.sender], "You have already voted.");
        require(_candidateId > 0 && _candidateId <= candidatesCount, "Invalid candidate ID.");

        // Record the vote
        voters[msg.sender] = true;
        candidates[_candidateId].voteCount++;
        totalVotes++;

        // Emit event
        emit Voted(msg.sender, _candidateId);
    }

    // Function to get the vote count of a candidate
    function getVoteCount(uint _candidateId) public view returns (uint) {
        require(_candidateId > 0 && _candidateId <= candidatesCount, "Invalid candidate ID.");
        return candidates[_candidateId].voteCount;
    }

    // Function to get the total number of votes
    function getTotalVotes() public view returns (uint) {
        return totalVotes;
    }
}
