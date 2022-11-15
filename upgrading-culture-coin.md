# Upgrading Culture Coin

Upgrading Culture Coin

On June 30th, 2022, it was brought to the library’s attention that CultureCoin.sol had a devastating flaw requiring an upgrade. Because of the nature of the pattern used, that of a transparent upgradable proxy, it should be possible to fix the flaw without a hard fork of the entire site.

The issue can be seen at: [CultureCoin.sol#L426](https://github.com/johnrraymond/greatlibraryserver/blob/main/brownie/contracts/CultureCoin.sol#L426). This recover() function has the flaw that anyone can empty the coin’s dex, leading to withdrawal problems. Luckily no funds can be stolen from the library because the code transfers all dex coins to the Culture Coin Administrator wallet.

However, this flaw is critical enough to require an upgrade. Therefore, this document exists to track this upgrade and all subsequent upgrades made to Culture Coin.

### How to Proceed <a href="#_buw2mrfommzd" id="_buw2mrfommzd"></a>

Step one was to identify the bug which happened on and around June 30th, 2022. The second step is to create a patch to the solidity code and to ready the new contract for deployment.

Of note, the Deadalus Class also has a similar problem but is less severe given that very little CC/AVAX/XMTSP should be held in Daedalus contracts. However, they all will require a similar upgrade, but with a “harder” fork. They do not use the proxy pattern. For more information see [DaedalusClass.sol#L57](https://github.com/johnrraymond/greatlibraryserver/blob/main/brownie/contracts/DaedalusClass.sol#L57).

### Modifying Culture Coin’s Implementation <a href="#_p5p1mfdbeyvw" id="_p5p1mfdbeyvw"></a>

Based on [ChainShot's how-to-make-contracts-upgradeable](https://www.chainshot.com/article/how-to-make-contracts-upgradeable) it appears that all that is needed is 1) to copy the contract to a new name and rewrite this function, 2) then deploy the new contract with the new Culture Coin implementation, and 3) tell the proxy/proxy admin about the new contract address for the new implementation/the cultureCoinImplAddress.

The initializer function can stay the same as it will not be called. However, to open up space and lower deployment costs it should be possible to remove all the code inside the initializer function and to leave it empty in all subsequent versions as it can only be called once in Culture Coin’s lifetime if we want to avoid a hard fork of the entire library site.

### Doing the Upgrade <a href="#_6riqpeymejpm" id="_6riqpeymejpm"></a>

1. Push new impl and save the address to be cultureCoinImplAddress in .env file.

brownie run scripts/deployCultureCoinV1M0m1.py --network=avax-test

1. Run the upgrade script.

brownie run scripts/upgradeCultureCoin.py --network=avax-test
