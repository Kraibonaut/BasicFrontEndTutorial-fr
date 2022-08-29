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

La première étape consiste à créer une page HTML de base.

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

7.  À l'intérieur de la balise div, ajoutez des boutons.

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
   - Vous n'avez jamais utilisé Remix ? Consultez le site [This video](https://www.youtube.com/watch?v=pdJttvcAV1c) 
2. Go to [Remix](https://remix.ethereum.org)
3. Vérifiez les onglets "Solidity Compiler", et "Deploy and Run Transactions". S'ils ne sont pas présents, activez-les dans le gestionnaire de plugins. 
4. Créez un nouveau fichier solidity dans remix, nommé `mood.sol`.
5. Ecrivez le contrat

   - Spécifier la version de solidity et ajouter une licence

   ```
   // SPDX-License-Identifier: MIT
    pragma solidity ^0.8.1;
   ```

   - Définissez le contrat

   ```
    contract MoodDiary{

    }
   ```

   - Dans le contrat, créez une variable appelée mood

   ```
    string mood;
   ```

   - Ensuite, créez les fonctions de lecture et d'écriture

   ```
    //créer une fonction qui écrit une humeur au smart contract
    function setMood(string memory _mood) public{
        mood = _mood;
    }

    //créer une fonction qui lit l'humeur du contrat intelligent
    function getMood() public view returns(string memory){
        return mood;
    }
   ```

   - Et voilà, c'est fait ! Votre code devrait ressembler à [ça](https://github.com/LearnWeb3DAO/BasicFrontEndTutorial/blob/master/contracts/mood.sol)

6. Déployez le contrat sur le Ropsten Testnet.
   - Assurez-vous que votre Metamask est connecté au Ropsten Testnet.
   - Assurez-vous que vous sélectionnez la bonne version du compilateur pour correspondre au contrat Solidity. (Dans l'onglet compiler)
   - Compilez le code en utilisant l'onglet "Solidity Compiler". Notez que le chargement du compilateur peut prendre un certain temps.
   - Déployez le contrat sous l'onglet "Deploy and Run Transactions".
   - Sous la section "Deployed Contracts", vous pouvez tester vos fonctions dans l'onglet "Remix Run" pour vous assurer que votre contrat fonctionne comme prévu !

**_Assurez-vous de déployer sur Ropsten via Remix sous l'environnement `Injected Web3` et confirmez la transaction de déploiement dans Metamask._**

Créez un nouveau fichier temporaire à conserver :

- L'adresse du contrat déployé
- Copiez-la via le bouton de copie situé à côté de la liste déroulante des contrats déployés dans l'onglet **Run** de Remix.
- L'ABI du contrat ([qu'est-ce que c'est ?](https://solidity.readthedocs.io/en/develop/abi-spec.html))
- Copiez-le via le bouton de copie situé sous le contrat dans l'onglet **Compile** de remix (également dans Détails).

---

### Connectez votre page Web à votre contrat intelligent

De retour dans votre éditeur de texte local, dans `index.html`, ajoutez le code suivant à votre page html :

1. Importez la source Ethers.js dans votre page `index.html` à l'intérieur d'un nouvel ensemble de balises de script:

```html
<script
  src="https://cdn.ethers.io/lib/ethers-5.2.umd.min.js"
  type="application/javascript"
></script>

<script>
  ////////////////////
  //AJOUTEZ VOTRE CODE ICI
  ////////////////////
</script>
```

2. À l'intérieur de la balise script, importez le contrat ABI ([what is that?](https://solidity.readthedocs.io/en/develop/abi-spec.html)) et spécifier l'adresse du contrat sur la blockchain de notre fournisseur :

```javascript
  const MoodContractAddress = "<contract address>";
  const MoodContractABI = <contract ABI>
  let MoodContract;
  let signer;
```

Pour le contrat ABI, nous voulons spécifiquement naviguer vers le fichier [JSON Section (https://docs.soliditylang.org/en/develop/abi-spec.html#json).
Nous devons décrire notre contrat intelligent au format JSON.

Puisque nous avons deux méthodes, cela devrait commencer comme un tableau, avec 2  objects:

```
const MoodContractABI = [{}, {}]
```

A partir de la page ci-dessus, chaque objet devrait avoir les champs suivants : `constant`, `inputs`, `name`, `outputs`, `payable`, `stateMutability` and `type`.

Pour `setMood`, nous décrivons chaque champ ci-dessous:

- name: `setMood`, s'explique lui-même
- type: `function`, s'explique lui-même
- outputs: devrait être `[]` parce que cela ne retourne rien
- stateMutability: Ceci est `nonpayable` car cette fonction n'accepte pas Ether
- inputs: this is an array of inputs to the function. Each object in the array should have `internalType`, `name` and `type`, and these are `string`, `_mood` and `string` respectively

pour `getMood`, nous décrivons chaque champ ci-dessous:

- name: `getMood`, s'explique lui-même
- type: `function`, s'explique lui-même
- outputs: ce dernier a le même type que `inputs` dans `setMood`. Pour `internalType`, `name` et `type`, cela devrait être `string`, `""`, et `string` respectivement
- stateMutability: C'est `view` parce que c'est une fonction de view.
- inputs: ceci n'a pas d'arguments donc cela devrait être `[]`

Votre résultat final devrait ressembler à ceci :

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

3. Ensuite, définissez un fournisseur d'ethers. Dans notre cas, c'est Ropsten :

```javascript
const provider = new ethers.providers.Web3Provider(window.ethereum, "ropsten");
```

4. Demandez l'accès au portefeuille de l'utilisateur et connectez le signataire à votre compte metamask (nous utilisons `[0]` par défaut), et définissez l'objet du contrat en utilisant votre adresse de contrat, ABI, et le signataire.

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

5. Créez des fonctions asynchrones pour appeler les fonctions de votre contrat intelligent.

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

6. Reliez vos fonctions à vos boutons html

```html
<button onclick="getMood()">Get Mood</button>
<button onclick="setMood()">Set Mood</button>
```

---

### Testez votre exercices !

1. Votre serveur web est en place ? Aller à [http://127.0.0.1:3000/](http://127.0.0.1:3000/) dans votre navigateur pour voir votre page !
2. Testez vos fonctions et approuvez les transactions si nécessaire via Metamask. Notez que les temps de bloc sont de ~15 secondes... donc attendez un peu pour lire l'état de la blockchain.
3. Consultez votre contrat et les informations sur les transactions via [https://ropsten.etherscan.io/](https://ropsten.etherscan.io/)
4. Ouvrez une console (`Ctrl + Shift + i`) dans le navigateur pour voir la magie se produire lorsque vous appuyez sur ces boutons.

---

### C'EST FAIT !

Célébrez ! Vous venez de créer une page web qui interagit avec _un véritable réseau de test Ethereum sur Internet_ ! Ce n'est pas quelque chose que beaucoup de gens peuvent dire qu'ils ont fait !

---

### Si vous avez eu des difficultés avec le tutoriel, vous pouvez essayer l'application d'exemple fournie.

```bash
git clone https://github.com/LearnWeb3DAO/BasicFrontEndTutorial.git
cd BasicFrontEndTutorial
lite-server
```

#### Essayez d'utiliser les informations suivantes pour interagir avec un contrat existant que nous avons publié sur le Roptsen testnet :

- Nous avons créé une instance de contrat `MoodDiary`. [at this transaction](https://ropsten.etherscan.io/tx/0x8da093fdc4ae3e1b469dfff97b414a9800c9fdd8c1c897b6b746faf43aa3b7f8)

- Voici le contrat en question ([on etherscan](https://ropsten.etherscan.io/address/0xc5afd2d92750612a9619db2282d9037c58fc22cb))

- Nous avons également vérifié notre code source pour [ropsten.etherscan.io](https://ropsten.etherscan.io/address/0xc5afd2d92750612a9619db2282d9037c58fc22cb#code) comme une mesure supplémentaire pour vous de vérifier ce que le contrat est exactement, et aussi l'ABI est disponible pour _le monde_ !

- L'ABI est aussi dans [this file](https://github.com/LearnWeb3DAO/BasicFrontEndTutorial/blob/master/Mood_ABI.json)

#### Cela illustre un point important : vous pouvez également construire une dApp _sans avoir besoin d'écrire le contrat Ethereum vous-même_ ! Si vous voulez utiliser un contrat existant écrit et déjà sur Ethereum, vous le pouvez !
