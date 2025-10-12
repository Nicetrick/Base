// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract TenRegister {
    address public owner;
    uint256 public constant MAX_REG = 10;

    address[] private registered;
    mapping(address => bool) private isReg;
    mapping(address => uint256) public deposits;

    event Registered(address indexed who);
    event Deposit(address indexed who, uint256 amount);
    event OwnerWithdraw(address indexed to, uint256 amount);

    modifier onlyOwner() {
        require(msg.sender == owner, "only owner");
        _;
    }

    constructor() {
        owner = msg.sender;
    }

    function register() external {
        require(!isReg[msg.sender], "already registered");
        require(registered.length < MAX_REG, "max reached");
        isReg[msg.sender] = true;
        registered.push(msg.sender);
        emit Registered(msg.sender);
    }

    // view helpers
    function isRegistered(address _a) external view returns (bool) {
        return isReg[_a];
    }

    function getRegistered() external view returns (address[] memory) {
        return registered;
    }

    // allow registered users to deposit small amounts
    function deposit() external payable {
        require(isReg[msg.sender], "only registered");
        require(msg.value > 0, "no zero");
        deposits[msg.sender] += msg.value;
        emit Deposit(msg.sender, msg.value);
    }

    // owner can withdraw whole balance
    function ownerWithdraw(address payable _to) external onlyOwner {
        uint256 bal = address(this).balance;
        require(bal > 0, "empty");
        (bool ok, ) = _to.call{value: bal}("");
        require(ok, "transfer failed");
        emit OwnerWithdraw(_to, bal);
    }

    // fallback receive
    receive() external payable {
        // accept plain transfers (but they won't count as deposit)
    }
}
