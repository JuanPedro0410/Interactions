# Interactions.s.sol
```
contract FundFundMe is Script {
    uint256 SEND_VALUE = 0.01 ether;

    function fundFundMe(address mostRecentlyDeployed) public {
        vm.startBroadcast();
        FundMe(payable(mostRecentlyDeployed)).fund{value: SEND_VALUE}();
        vm.stopBroadcast();
    }

    function run() external {
        address mostRecentlyDeployed = DevOpsTools.get_most_recent_deployment(
            "FundMe",
            block.chainid
        );

        fundFundMe(mostRecentlyDeployed);

        console.log("FundMe balance!");
    }
}
```
Fund The Last deployed FundMe with the value stored in SEND_VALUE that is 0.1 ether.
Get the las contract deployed Usign the function DevOpsTools.get_most_recent_deployment("FundMe", block.chainid ) that you must to pass the name of the contract and the chain id, then it returns the address of the last deployed contract. And then execute the function that fund the las deployed contract.


```
contract WithdrawFundMe is Script {
    function withdrawFundMe(address mostRecentlyDeployed) public {
        vm.startBroadcast();
        FundMe(payable(mostRecentlyDeployed)).withdraw();
        vm.stopBroadcast();
        console.log("Withdraw FundMe balance!");
    }

    function run() external {
        address mostRecentlyDeployed = DevOpsTools.get_most_recent_deployment(
            "FundMe",
            block.chainid
        );

        withdrawFundMe(mostRecentlyDeployed);
    }
}
```

withdraw the last deployed contract.
Get the las contract deployed Usign the function DevOpsTools.get_most_recent_deployment("FundMe", block.chainid ) that you must to pass the name of the contract and the chain id, then it returns the address of the last deployed contract. And then execute the function that withdraw the las deployed contract.


# Interactions.t.sol

```
    FundMe private s_fundMe;
    HelperConfig private helper_config;

    uint256 public constant SEND_VALUE = 0.1 ether; // just a value to make sure we are sending enough!
    uint256 public constant STARTING_USER_BALANCE = 10 ether;
    uint256 public constant GAS_PRICE = 1;

    address public constant USER = address(1);
```

Create the main variables.

```
    function setUp() external {
        DeployFundMe deploy = new DeployFundMe();
        (s_fundMe, ) = deploy.run();
        vm.deal(USER, STARTING_USER_BALANCE);
    }
```

create a new fundMe contract and give to a local fake address called USER the amount stored in STARTING_BALANCE

```
    function testUserCanFundInteractions() public {
        FundFundMe fundFundMe = new FundFundMe();
        fundFundMe.fundFundMe(address(s_fundMe));

        WithdrawFundMe withdrawFundMe = new WithdrawFundMe();
        withdrawFundMe.withdrawFundMe(address(s_fundMe));

        assert(address(s_fundMe).balance == 0);
    }
```
create a new test that deploy a new FundFundMe contract that is the one that we created on Interactions.sol ans then execute the function fundFundMe that fund the fundMe.
Then i do the same bu twith the WithdrawContract and withdraw the fundMe.
Then do and Axssert to check if the balance of the contract is zero.
I forgoten to write that when you execute fundFundMe and withdrawFundMe you must to pass the address of the fundme That we created on setUp().
