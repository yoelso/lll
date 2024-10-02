# lll
Proyecto: Foundry-Bank-Contract


Contrato: Bank.sol

solidity
pragma solidity ^0.8.0;

contract Bank {
    mapping(address => uint256) public balances;

    function deposit() public payable {
        balances[msg.sender] += msg.value;
    }

    function withdraw(uint256 amount) public {
        require(balances[msg.sender] >= amount);
        balances[msg.sender] -= amount;
        payable(msg.sender).transfer(amount);
    }
}



Pruebas: Bank.t.sol

solidity
pragma solidity ^0.8.0;

import "foundry/Foundry.sol";
import "../Bank.sol";

contract BankTest is Foundry {
    Bank public bank;

    function setUp() public {
        bank = new Bank();
    }

    function testDeposit() public {
        bank.deposit{value: 10 ether}();
        assertEq(bank.balances(address(this)), 10 ether);
    }

    function testWithdraw() public {
        bank.deposit{value: 10 ether}();
        bank.withdraw(5 ether);
        assertEq(bank.balances(address(this)), 5 ether);
    }

    // Pruebas fuzz
    function testFuzzDeposit(uint256 amount) public {
        bank.deposit{value: amount}();
        assertEq(bank.balances(address(this)), amount);
    }

    function testFuzzWithdraw(uint256 amount) public {
        bank.deposit{value: amount}();
        bank.withdraw(amount / 2);
        assertEq(bank.balances(address(this)), amount / 2);
    }
}



Script de implementación: deploy.sol

solidity
pragma solidity ^0.8.0;

import "../Bank.sol";

contract Deploy {
    function deployBank() public {
        Bank bank = new Bank();
        // Implementación en diferentes redes
        if (blockchain.network() == "mainnet") {
            // Implementación en Mainnet
        } else if (blockchain.network() == "rinkeby") {
            // Implementación en Rinkeby Testnet
        }
    }
}



Clase para administrar redes: NetworkManager.sol

solidity
pragma solidity ^0.8.0;

contract NetworkManager {
    mapping(string => address) public networkAddresses;

    function addNetwork(string memory _network, address _address) public {
        networkAddresses[_network] = _address;
    }

    function getNetworkAddress(string memory _network) public view returns (address) {
        return networkAddresses[_network];
    }
}



EXPLICACION

# Foundry-Bank-Contract

Proyecto de Foundry con contrato de muestra, cobertura de pruebas completa y pruebas fuzz.

## Utilidad de las pruebas fuzz

Las pruebas fuzz permiten probar el contrato con diferentes valores de entrada, lo que ayuda a detectar errores y vulnerabilidades.

## Configuración de los scripts de implementación

Los scripts de implementación se configuran en el archivo `deploy.sol`. Se utiliza la clase `NetworkManager` para administrar las diferentes redes.

## Comandos

* `forge build`: Compilar el contrato
* `forge test`: Ejecutar pruebas
* `forge deploy`: Implementar el contrato en diferentes redes



