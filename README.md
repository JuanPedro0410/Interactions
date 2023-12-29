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
