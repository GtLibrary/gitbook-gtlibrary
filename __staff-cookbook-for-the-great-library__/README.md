# \_\_Staff Cookbook for the Great Library\_\_

::Staff Cookbook for The Great Library::

::For Staff Use Only::

### Howto: Mint Hardbound, Bookmark, and Book NFTs <a href="#_h7hxe8xknzmx" id="_h7hxe8xknzmx"></a>

1. Verify the secure backend is working. In the test environment that means making sure the testbackend.greatlibrary.io machine is working.
2. <img src="../.gitbook/assets/0 (1)" alt="" data-size="original">
   1. The testbackend expects url’s in this form. Note that the whofile is unused currently.
      1. /0x0/ is the current contractid, and is 0x0 always when for new book contracts.
      2. Name is the long name, like BMTLSC for bookmark’s for The Lightshy Crow.
      3. Symbol is the same as name.
      4. Marketplaceaddress is the address of the market\_place.sol’s main contract: MarketPlace. This is the same as the bookregistryaccount.
      5. Metadata-uri is the url of where the images for the bookmarks are stored.
      6. Burnable is a true or false
      7. Maxmint is the number of bookmarks the printing press is allowed to print out.
      8. Defaultprice is price in AVAX.
      9. Mintto is the author’s wallet address.
   2. Once all three main contracts, HB, BT, and BM, are created this way:
      1. The rewards are verified to make sure when someone buys a bookmark they automatically receive a book as well.
      2. ![](<../.gitbook/assets/1 (1)>)
         1. The hierarchy is BookmarkContractIds contain—or are the parent of—BookContractIds
   3. See [https://github.com/GtLibrary/thegreatlibrary/blob/main/moralis/secure-backend.js](https://github.com/GtLibrary/thegreatlibrary/blob/main/moralis/secure-backend.js)
   4. This part is tricky: newBookContract() in the smart contract itself:
      1. ![](<../.gitbook/assets/2 (1)>)
   5. On line 141 the new HB, or BT, or BM contracts are transfered.
   6. The ownership of the contract, not the tokens, but the actual erc721 contract is transferred to the \_mintTo:
      1. \_mintTo is a bad name, because minting implies tokens and this isn't minting tokens, it is making contracts. The function newBookContract() is technically a factory.
      2. From author page:
         1. Author calls newBookContract() with \_mintTo equal to the cCA.
         2. Author then tells the website to transfer the ownership to the author's wallet.
         3. Website backend calls into the secure-backend.js.
            1. The backend does the transfer.
         4. Once any of the three contracts are made, they need to be connected to the site’s book page.

### Howto: Understand the NFTs and AI Play <a href="#_yk5bmnx1zpkz" id="_yk5bmnx1zpkz"></a>

1. Bookmarks are based on text.
2. GPT-3 prompts are based on text.
3. The simplest possible Smart Book is a list of GPT-3 prompt texts
   1. Take our cat BEN as an example.
      1. Each token has a personality.
         1. Smart BEN, Silly BEN, Little BEN, Big BEN, Happy BEN, Dumb BEN, Rich BEN, Poor BEN, Lucky BEN, Unlucky BEN, Greedy BEN, Benevolent BEN, Evil BEN, Etc.
         2. These personalities can be converted to GPT-3 prompts and used in multiple ways.
            1. One way to use the prompts is to use them bare and with some randomness to keep the results fresh.
            2. Another way is to use the prompts to help couch questions and other feedback where the user “talks” with the avatar.
            3. These responses then form a new token, and with the case of BEN, they form The Great Book of Scratches.

Users are given a token from The Great Book of Scratches for each response they request.

*
  1. Take the Large Cat, AKA The Tiger Series, as another example
     1. Each token has a personality, and an image.
        1. Price to purchase
        2. Price to greet the tiger
        3. Price to bring tiger ingame as pet
        4. Price to ask tiger a question
     2. The response tokens come from “The Tiger Series”
        1. Response tokens are:
           1. Burnable
           2. Tradeable
           3. Upgradeable
           4. Metadata include

A smaller, AI/Stable Diffusion version of the original image of the tiger

### Howto: Bridging, Hosting, and the “Great Marketplace’s” NFT “Transporter Technology” <a href="#_a2t97wp5hqxz" id="_a2t97wp5hqxz"></a>

1. Bridging is the most straightforward way to consolidate marketplaces
   1. The marketplace can be chain agnostic in its framework
      1. There is a switcher function in the ui that lets the users browse by chains
      2. There is an import/export (able) button (and flag) on each nft shown
         1. The flag will show if an NFT is importable to a chosen chain
         2. The button will trigger the movement of the NFT (and exportable)
            1. When the NFT is owned by the user

It is transferred by the user (or superuser user depending on the bridge) to the exporter contract of the “Transporter Technology.” The Exporter side holds two key data:

Where to look for the NFT

What wallet address can claim it there

And it should emit at least one event.

The event should emit the where and what of above.

The bridge is alerted/watches the state and senses a change has occurred on the network.

The bridge then, looking at the event data, calls into the Importer side contract:

This causes the NFT locked in a local contract on the new network to be emitted to the claimant’s wallet address.

“Unlocking” could be minting _de novo_ if the NFT’s ERC-styling allows for it.

Or if in the case of an ERC721, it might mean a complete copy of the minting up to the token must be done on the receiving network first.

80 Dollars or so to do a deploy and a mint on Avalanche per 1000 tokens.

But should also cost next to nothing for the free customer.

1. Hosting of NFT projects, not just those of the library’s, will be possible using a scheme like the above because it lets the library do all the bridging work for them.
2. All first-order hops will lead directly to the Avalanche C-Chain.
   1. This lets all NFTs governed by the system do the “AVAX Hop”
      1. For example, you have an NFT collection on Ethereum Mainnet but one of the token owners wants to hold it on the Avalanche Network so he and his friend can pass it around more cheaply.
      2. Or the user wants to sell the NFT and best way is directly on the “Great Marketplace” found on the Avalanche Network
      3. Mirror mintings allow updated contract logic without requiring forks on the original network. Updated metadata and logic may include
         1. Gas tokens
         2. Metadata from Artificial Intelligence and Arthouses
            1. Image

Dalle-2

*
  *
    *
      *
        1. Movie

Kino?

*
  *
    *
      *
        1. Sound

Text to Speech

*
  *
    *
      *
        1. Text

GPT-3

*
  *
    *
      *
        1. Etc.

### Howto: Install the Demo: <a href="#_nxluv8tpd25f" id="_nxluv8tpd25f"></a>

### Howto: Be a Culture Coin Whale: <a href="#_8515tu1kda1v" id="_8515tu1kda1v"></a>

1. While it is possible to use the CC dex as your own personal money machine, but because of the sheer numbers involved, 1) you never dex out your Culture Coin if you are staff or have ever been staff or if you are a whale. Crashing the dex helps no one.
   1. If you cannot agree to this “first provision,” you cannot be a staff member with the great library.
2. 2\) This doesn’t mean you can’t dip in and take a small amount as long as you put it back before it is noticed missing. This means loans from the dex or using the dex is fine as long as you aren’t collapsing the dex.
3. 3\) If it feels wrong, don’t do it.
   1. This should go without saying.
4. 4\) The primary mission for Culture Coin, CC, $CC, etc, is to act as the library’s gas token and to function as a payment method.
   1. The author ERC721Tradables, BookTradables, are largely priced in AVAX so they are a way to generate AVAX from CC.
      1. Use your CC to “gas up” your favorite BookTradables and see just how much life you can pump into them.
   2. You can sell the BookTradable you own for a profit on the open market.
      1. This is a liquidity event for all staff and whales with “good” nfts.
         1. Truth is all staff picks are going to be winners.
            1. The Lightshy Crow and the four books of The Scarab Cycle.
            2. In the Beginning… and first four books of The Runners!
            3. The Count of Monte Carlo, Alice and Wonderland,
            4. Etc, any book in the public domain that looks good can become part of the “living library.”
5. 5\) As staff, or as a whale, you have to think of yourself as an oil baron, one who wants to keep the price of oil up. Or in this case, the price of Culture Coin up.

### Howto: Hack the Artificial Intelligence Pipeline <a href="#_j2u6o74joquj" id="_j2u6o74joquj"></a>

1. The point of our pipeline is to use Dalle-2 to illustrate the books
2. 1\) Need the original text from the book in question,
3. 2\) Need the pre-prompt text to initiate “main style” system
   1. “In this David versus Goliath, David is darkskinned like his Aramaic brothers; and Goliath is a tall, whip of a man, obscured by shadows as a fatal moon hangs them both as they fight, oblivion to one side and good on the other.” \[EDITOR: Do not fix this text. Please delete it and replace it with a better example here and below.]
   2. Results:
      1. ![](<../.gitbook/assets/3 (2)>)![](../.gitbook/assets/4)![](<../.gitbook/assets/5 (1)>)![](../.gitbook/assets/6)![](<../.gitbook/assets/7 (1)>)![](<../.gitbook/assets/8 (1)>)![](<../.gitbook/assets/9 (1)>)![](../.gitbook/assets/10)![](../.gitbook/assets/11)![](../.gitbook/assets/12)![](<../.gitbook/assets/13 (1)>)![](<../.gitbook/assets/14 (1)>)![](<../.gitbook/assets/15 (1)>)
4. Gpt-3 finetuning/scripting template language example:
   1. Run script load\_scarab\_cycle\_universe.exe
   2. Run script rennly\_and\_gaz.exe
   3. The sun had long since set in the hours since leaving the homestead. Shadows now hung in the air above the ledge separating Gaz from his bed. Ghosts were said to inhabit such cracks. Now in the dark, Gaz was too afraid to disbelieve. “Luckily, I been smart,” he muttered to himself. He had used the last of his coin to buy a whiff of tarsk to keep him company on the way home. Uncorking the bottle, he took a quick swig. “Hah!” he said as he slammed the cork down the throat of the jug. Emboldened, he took a step forward. A shadow loomed up in front of him. He clutched the jug to his chest with both hands. “Ahh!”
   4. “I see you, Gaz,” the shadow said.
   5. Gaz stepped backwards against the wall of the cliff. “Wh-who is it?”
   6. The moon blinded him for a second but the creature resolved into Rennly Rickot. “Don’t you recognize me?” Rennly said.
   7. “Rennly,” Gaz said. “You’re back. And you are like… You.” He stepped forward out onto the ledge, pushing his jug of tarsk towards Rennly’s dark outline. “The moon really sells it I guess!”
   8. “And now, I’m not just here for visiting either,” Rennly said, his arms folded in front of him. He seemed stiffer, yet the same Rennly as ever.
   9. “I heard you were a censor now,” Gaz said. He struggled not to slur his words. “And I know I have to do better—when others are around—but look at you, all in black.” Gaz wanted to be proud of Rennly, but Rennly wasn’t his friend really, was he? “Why are you here?” he asked. “It’s the middle of nowhere.”
   10. Rennly finally relaxed his stance. “To tidy up some loose ends.”
   11. Run script dalle2.exe
   12. \*In this classical David versus Goliath painting, David is dark-skinned, similar to his Aramaic brothers. Goliath however is tall and obscured by moon shadows.
   13. These are old results for older prompts: ![](<../.gitbook/assets/16 (1)>)![](<../.gitbook/assets/17 (1)>)![](../.gitbook/assets/18)
   14. Run script reset\_universe.exe
   15. Run script load\_scarab\_cycle\_universe.exe
   16. …
5. \* In this part the “dalle-2 text” is the developer work product. It needs to fit within the dalle-2 api parameters and also must generate a decent Dalle-2 rendition of the scene for the book.
6. How this works is by creating a script of inputs and outputs for the “executables.” Using them as the mnemonic is a better system than using an xml/html bracket system. And “scripting” is much easier to understand by the users creating the first scripts.
7. The easiest way to think about this script is to imagine the computer is able to guess what these executables would be outputting. Clearly these .exe’s are real: they are outputting these amazing text prompts for Dalle-2.
8. Or think of it this way: the system learns by example so just repeat this exercise with various parts of the book and with good Dalle-2 prompts which work for them, then we will have a working AI pipeline.
9. Useful prompts and Dalle-2 results
   1. ??? ?? ?

### Howto: Mine Culture Coin for Fun and Profit <a href="#_ar0ikfqg35ww" id="_ar0ikfqg35ww"></a>

1. Step one is play the game
   1. Kill mobs
   2. Loot mobs
   3. Create Items from loot
   4. Sell items on the in-game auction house for CC
2. Convert CC from 1(d) into AVAX or other currency at the dex(es)
3. Profit
