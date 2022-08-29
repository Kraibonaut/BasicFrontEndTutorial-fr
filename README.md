# Level 7 - Créer une application Ethereum avec Ethers.js

### (Forked from [BlockDevsUnited/BasicFrontEndTutorial](https://github.com/BlockDevsUnited/BasicFrontEndTutorial))

Il s'agit d'un tutoriel étape par étape sur la façon de créer un front-end, de déployer un contrat intelligent Solidity et de les connecter ensemble.
Nous allons utiliser [Metamask](https://metamask.io), [Remix IDE](https://remix.ethereum.org) et [Ethers.js](https://github.com/ethers-io/ethers.js/).

À la fin de ce tutoriel, vous serez en mesure de créer un front-end HTML simple avec des boutons qui peuvent interagir avec les fonctions des contrats intelligents. Le tutoriel se déroule en 3 étapes

- Créer une page web HTML de base
- Créer un contrat intelligent Solidity de base
- Connecter la page web avec les contrats intelligents en utilisant Ethers.js

---

## Vous préférez une vidéo ?

Si vous préférez apprendre à partir d'une vidéo, nous avons un enregistrement disponible de ce tutoriel sur notre YouTube. Regardez la vidéo en cliquant sur la capture d'écran ci-dessous, ou lisez le tutoriel!

[![Cryptocurrency Tutorial](https://i.imgur.com/pDcYqIg.png)](https://www.youtube.com/watch?v=aqxAWLi6UMA "dApp Tutorial")

### Preparation

1. **Télécharger et installer [MetaMask](https://metamask.io)**

   - Vous n'avez jamais utilisé Metamask ? Regardez [this explainer video](https://youtu.be/wlm4QcA8c4Q?t=66)

     _Les éléments importants : `1:06 to 4:14`_

   - Cliquez sur Ethereum Mainnet en haut. Passez au Ropsten Tesnet et obtenez une copie de l'adresse publique du compte sur votre Metamask Wallet.


2. **Demandez de l'éther Ropsten Tesnet à partir d'un robinet/faucet chargé dans votre porte-monnaie Metamask.**

   - [Faucet link to request funds](https://faucet.egorfine.com/)
   - [Blog explaining a faucet and how to use one](https://blog.b9lab.com/when-we-first-built-our-faucet-we-deployed-it-on-the-morden-testnet-70bfbf4e317e)

3. **Installez un serveur http. Utilisez celui que vous voulez, mais nous recommandons `lite-server` pour les débutants :**

   - Install Node.js ([Download and Instructions](https://nodejs.org/en/download/))
   - Install lite-server (with NPM in a terminal/command prompt):

   ```bash
   npm install -g lite-server #install lite-server globally
   ```

---

### Créer et utiliser une page Web simple

The first step is to create a basic HTML page.

1.  Créez un nouveau dossier (répertoire) dans votre terminal en utilisant `mkdir <nom du répertoire>`.
2.  Dans un éditeur de code (par exemple Atom, ou Visual Studio Code), ouvrez le dossier
3.  Créer un nouveau fichier appelé `index.html`.
4.  Ouvrez index.html
5.  Créer un modèle HTML

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>My First dApp</title>
  </head>
  <body></body>
</html>
```

Nous allons créer une application qui lit et écrit simplement une valeur sur la blockchain. Nous aurons besoin d'une étiquette, d'une entrée et de boutons.

6. À l'intérieur de la balise body, ajoutez du texte, une étiquette et une entrée.

```html
<body>
  <div>
    <h1>This is my dApp!</h1>
    <p>Here we can set or get the mood:</p>
    <label for="mood">Input Mood:</label> <br />
    <input type="text" id="mood" />
  </div>
</body>
```

7.  Inside the div tag add some buttons.

```html
<button onclick="getMood()">Get Mood</button>
<button onclick="setMood()">Set Mood</button>
```

OPTIONNEL : A l'intérieur de la balise `<head>`, ajoutez quelques styles pour le rendre plus joli.

```html
<style>
  body {
    text-align: center;
    font-family: Arial, Helvetica, sans-serif;
  }

  div {
    width: 20%;
    margin: 0 auto;
    display: flex;
    flex-direction: column;
  }

  button {
    width: 100%;
    margin: 10px 0px 5px 0px;
  }
</style>
```

8.  Servez la page web via un terminal/une invite de commande à partir du répertoire qui contient `index.html` et exécutez :
    ```bash
    lite-server
    ```

9.  Aller à [http://127.0.0.1:3000/](http://127.0.0.1:3000/) dans votre navigateur pour voir votre page !

10. Votre front-end est maintenant complet !

---

### Créer un Smart Contract de base

Il est maintenant temps de créer un contrat intelligent Solidity.

1. Vous pouvez utiliser l'éditeur de votre choix pour réaliser le contrat. Pour ce tutoriel, nous recommandons l'IDE en ligne [Remix](https://remix.ethereum.org)
   - Never used Remix before? Checkout [This video](https://www.youtube.com/watch?v=pdJttvcAV1c)
2. Go to [Remix](https://remix.ethereum.org)
3. Check out the "Solidity Compiler", and "Deploy and Run Transactions" tabs. If they are not present, enable them in the plugin manager
4. Create a new solidity file in remix, named `mood.sol`
5. Write the contract

   - Specify the solidity version and add a license

   ```
   // SPDX-License-Identifier: MIT
    pragma solidity ^0.8.1;
   ```

   - Define the contract

   ```
    contract MoodDiary{

    }
   ```

   - Inside the contract create a variable called mood

   ```
    string mood;
   ```

   - Next, create Read and Write functions

   ```
    //create a function that writes a mood to the smart contract
    function setMood(string memory _mood) public{
        mood = _mood;
    }

    //create a function the reads the mood from the smart contract
    function getMood() public view returns(string memory){
        return mood;
    }
   ```

   - And that's it! Your code should look like [this](https://github.com/LearnWeb3DAO/BasicFrontEndTutorial/blob/master/contracts/mood.sol)

6. Deploy the contract on the Ropsten Testnet.
   - Make sure your Metamask is connected to the Ropsten Testnet.
   - Make sure you select the right compiler version to match the solidity contract. (In the compile tab)
   - Compile the code using the "Solidity Compiler" tab. _Note that it may take a moment to load the compiler_
   - Deploy the contract under the "Deploy and Run Transactions" tab
   - Under the Deployed Contracts section, you can test out your functions on the Remix Run tab to make sure your contract works as expected!

**_Be sure to deploy on Ropsten via Remix under the `Injected Web3` environment and confirm the deployment transaction in Metamask_**

Make a new temporary file to hold:

- The deployed contract's address
  - Copy it via the copy button next to the deployed contracts pulldown in remix's **Run** tab
- The contract ABI ([what is that?](https://solidity.readthedocs.io/en/develop/abi-spec.html))
  - Copy it via the copy button under to the contract in remix's **Compile** tab (also in Details)

---

### Connect Your Webpage to Your Smart Contract

Back in your local text editor in `index.html`, add the following code to your html page:

1. Import the Ethers.js source into your `index.html` page inside a new set of script tags:

```html
<script
  src="https://cdn.ethers.io/lib/ethers-5.2.umd.min.js"
  type="application/javascript"
></script>

<script>
  ////////////////////
  //ADD YOUR CODE HERE
  ////////////////////
</script>
```

2. Inside the script tag, import the contract ABI ([what is that?](https://solidity.readthedocs.io/en/develop/abi-spec.html)) and specify the contract address on our provider's blockchain:

```javascript
  const MoodContractAddress = "<contract address>";
  const MoodContractABI = <contract ABI>
  let MoodContract;
  let signer;
```

For the contract ABI, we want to specifically navigate to the [JSON Section](https://docs.soliditylang.org/en/develop/abi-spec.html#json).
We need to describe our smart contract in JSON format.

Since we have two methods, this should start as an array, with 2 objects:

```
const MoodContractABI = [{}, {}]
```

From the above page, each object should have the following fields: `constant`, `inputs`, `name`, `outputs`, `payable`, `stateMutability` and `type`.

For `setMood`, we describe each field below:

- name: `setMood`, self explanatory
- type: `function`, self explanatory
- outputs: should be `[]` because this does not return anything
- stateMutability: This is `nonpayable` because this function does not accept Ether
- inputs: this is an array of inputs to the function. Each object in the array should have `internalType`, `name` and `type`, and these are `string`, `_mood` and `string` respectively

For `getMood`, we describe each field below:

- name: `getMood`, self explanatory
- type: `function`, self explanatory
- outputs: this has the same type as `inputs` in `setMood`. For `internalType`, `name` and `type`, this should be `string`, `""`, and `string` respectively
- stateMutability: This is `view` because this is a view function
- inputs: this has no arguments so this should be `[]`

Your end result should look like this:

```
const MoodContractABI = [
	{
		"inputs": [],
		"name": "getMood",
		"outputs": [
			{
				"internalType": "string",
				"name": "",
				"type": "string"
			}
		],
		"stateMutability": "view",
		"type": "function"
	},
	{
		"inputs": [
			{
				"internalType": "string",
				"name": "_mood",
				"type": "string"
			}
		],
		"name": "setMood",
		"outputs": [],
		"stateMutability": "nonpayable",
		"type": "function"
	}
]
```

3. Next, Define an ethers provider. In our case it is Ropsten:

```javascript
const provider = new ethers.providers.Web3Provider(window.ethereum, "ropsten");
```

4. Request access to the user's wallet and connect the signer to your metamask account (we use `[0]` as the default), and define the contract object using your contract address, ABI, and signer

```javascript
provider.send("eth_requestAccounts", []).then(() => {
  provider.listAccounts().then((accounts) => {
    signer = provider.getSigner(accounts[0]);
    MoodContract = new ethers.Contract(
      MoodContractAddress,
      MoodContractABI,
      signer
    );
  });
});
```

5. Create asynchronous functions to call your smart contract functions

```javascript
async function getMood() {
  const getMoodPromise = MoodContract.getMood();
  const Mood = await getMoodPromise;
  console.log(Mood);
}

async function setMood() {
  const mood = document.getElementById("mood").value;
  const setMoodPromise = MoodContract.setMood(mood);
  await setMoodPromise;
}
```

6. Connect your functions to your html buttons

```html
<button onclick="getMood()">Get Mood</button>
<button onclick="setMood()">Set Mood</button>
```

---

### Test Your Work Out!

1. Got your webserver up? Go to [http://127.0.0.1:3000/](http://127.0.0.1:3000/) in your browser to see your page!
2. Test your functions and approve the transactions as needed through Metamask. Note block times are ~15 seconds... so wait a bit to read the state of the blockchain
3. See your contract and transaction info via [https://ropsten.etherscan.io/](https://ropsten.etherscan.io/)
4. Open a console (`Ctrl + Shift + i`) in the browser to see the magic happen as you press those buttons

---

### DONE!

Celebrate! You just made a webpage that interacted with _a real live Ethereum testnet on the internet_! That is not something many folks can say they have done!

---

### If you had trouble with the tutorial, you can try out the example app provided.

```bash
git clone https://github.com/LearnWeb3DAO/BasicFrontEndTutorial.git
cd BasicFrontEndTutorial
lite-server
```

#### Try and use the following information to interact with an existing contract we published on the Roptsen testnet:

- We have a `MoodDiary` contract instance created [at this transaction](https://ropsten.etherscan.io/tx/0x8da093fdc4ae3e1b469dfff97b414a9800c9fdd8c1c897b6b746faf43aa3b7f8)

- Here is the contract ([on etherscan](https://ropsten.etherscan.io/address/0xc5afd2d92750612a9619db2282d9037c58fc22cb))

  - We also verified our source code to [ropsten.etherscan.io](https://ropsten.etherscan.io/address/0xc5afd2d92750612a9619db2282d9037c58fc22cb#code) as an added measure for you to verify what the contract is exactly, and also the ABI is available to _the world_!

- The ABI is also in [this file](https://github.com/LearnWeb3DAO/BasicFrontEndTutorial/blob/master/Mood_ABI.json)

#### This illustrates an important point: you can also build a dApp _without needing to write the Ethereum contract yourself_! If you want to use an existing contract written and already on Ethereum, you can!
