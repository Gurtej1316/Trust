# Trust
pragma solidity =0.8.1;

contract trust {


    struct Kid{
        uint amount;
        uint maturity;
        bool paid;
        
    }
    mapping (address => Kid) public kids;
    address public admin;
    constructor() {
        admin = msg.sender;
    }
    function addkid(address kid, uint timetomaturity) external payable {
        require(msg.sender == admin);
        require(kids[msg.sender].amount ==0);
        kids[kid] = Kid(msg.value, block.timestamp + timetomaturity, false);
    }
    function withdaw() external{
        Kid storage kid = kids[msg.sender];
        require(kid.maturity <= block.timestamp);
        require(kid.amount >0);
        require(kid.paid == false);
        kid.paid = true;
        payable(msg.sender).transfer(kid.amount);
        
    }
}
