// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

/**
 * @title Crop Insurance Smart Policy
 * @dev A blockchain-based crop insurance management system for farmers and insurers.
 */
contract CropInsuranceSmartPolicy {
    address public insurer;
    uint256 public policyCount;

    enum PolicyStatus { Active, Claimed, Settled, Rejected }

    struct Policy {
        uint256 id;
        address farmer;
        string cropType;
        uint256 premium;
        uint256 coverageAmount;
        PolicyStatus status;
    }

    mapping(uint256 => Policy) public policies;

    event PolicyCreated(uint256 policyId, address farmer, string cropType, uint256 premium);
    event ClaimFiled(uint256 policyId);
    event ClaimSettled(uint256 policyId, uint256 amount);
    event ClaimRejected(uint256 policyId);

    modifier onlyInsurer() {
        require(msg.sender == insurer, "Only insurer can perform this action");
        _;
    }

    constructor() {
        insurer = msg.sender;
        policyCount = 0;
    }

    // Function 1: Create new crop insurance policy
    function createPolicy(string calldata _cropType, uint256 _premium, uint256 _coverageAmount) external payable {
        require(msg.value == _premium, "Premium amount must be paid");
        policyCount++;
        policies[policyCount] = Policy(policyCount, msg.sender, _cropType, _premium, _coverageAmount, PolicyStatus.Active);
        emit PolicyCreated(policyCount, msg.sender, _cropType, _premium);
    }

    // Function 2: File a claim for crop loss
    function fileClaim(uint256 _policyId) external {
        Policy storage policy = policies[_policyId];
        require(msg.sender == policy.farmer, "Only farmer can file a claim");
        require(policy.status == PolicyStatus.Active, "Policy not active");
        policy.status = PolicyStatus.Claimed;
        emit ClaimFiled(_policyId);
    }

    // Function 3: Settle or reject claim by insurer
    function settleClaim(uint256 _policyId, bool _approve) external onlyInsurer payable {
        Policy storage policy = policies[_policyId];
        require(policy.status == PolicyStatus.Claimed, "Claim not filed");
        
  ##   Contracts details : 0xB6f643CDDf181FC3125c45d9E977093136B210ec
  <img width="1353" height="561" alt="Screenshot 2025-10-29 231532" src="https://github.com/user-attachments/assets/1735765f-72ef-4c2a-b3ee-5b48ce329cbd" />

