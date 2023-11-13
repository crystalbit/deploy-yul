# deploy-yul
Contracts for Foundry for deploying yul and bytecode contracts.

[Example foundry project](https://github.com/crystalbit/foundry-yul-boilerplate) using these contracts

[Instruction on YouTube](https://www.youtube.com/watch?v=lwvUiRIU_Po) 

## Installation to a Foundry project
1) Install this repo as git submodule
```
forge install git@github.com:crystalbit/deploy-yul.git
```
2) Update remappings (or do it manually the way you want):
```
forge remappings > remappings.txt
```

You will likely have these remappings after updating:
```
deploy-yul/=lib/deploy-yul/contracts/
ds-test/=lib/forge-std/lib/ds-test/src/
forge-std/=lib/forge-std/src/
```
3) Add ffi support into settings – `ffi = true` in `[profile.default]` section in `foundry.toml`. It allows to run external file (`solc` compiler in case of this module).  
My foundry.toml:
```toml
[profile.default]
src = "src"
out = "out"
libs = ["lib"]
ffi = true
```

## Deploying Yul
1) Create folder `yul` in the root of your project
2) Create contract with `.yul` extension there, for example `ContractName.yul`
3) Use this import in your deploy script or test:
```
import "deploy-yul/YulDeployer.sol";
```
4) Create yulDeployer in test contract/script state (after `contract` keyword and before functions):
```
YulDeployer yulDeployer = new YulDeployer();
```
5) Deploy in your deploy script or test like this:
```
address ContractName = yulDeployer.deployContract("ContractName");
```

## Deploying bytecode
1) Create folder `bytecode` in the root of your project
2) Create contract with `.hex` extension there, for example `ContractName.hex`
3) Use this import in your deploy script or test:
```
import "deploy-yul/BytecodeDeployer.sol";
```
4) Create bytecodeDeployer in test contract/script state (after `contract` keyword and before functions):
```
BytecodeDeployer bytecodeDeployer = new BytecodeDeployer();
```
5) Deploy in your deploy script or test like this:
```
address ContractName = bytecodeDeployer.deployContract("ContractName");
```

## Contract examples for testing:
1. Yul contract example:
```yul
object "Basic" {
  code {
    datacopy(0, dataoffset("Runtime"), datasize("Runtime"))
    return(0, datasize("Runtime"))
  }
  object "Runtime" {
    code {
      let sum := add(3, 5)
      mstore(0x0, sum)
      return(0x0, 0x20)
    }
  }
}
```

2. Bytecode file example:
```
600e600d600039600e6000f3fe600560030160005260206000f3

only first line of this file is used
you can write comments here
```
