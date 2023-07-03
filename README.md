# Module de paiement de KaliaPay

Un module de paiement pour intégrer KaliaPay dans votre application de manière simplifiée pour javascript.

## Préréquis

Pour utiliser ce module, il faut vous télécharger Axios.

#### 1. Avoir un compte kaliapay

Assurez-vous de disposer d'un compte Kaliapay ce qui permettra de pouvoir utiliser les services proposés.
Si vous ne l'avez pas, créez-en un sur : https://kaliapay.com/back-office/signup/ 
   
#### 2. Configuration des urls 

Configurer l'url de notification, l'url de retour et l'url d'annulation dans votre espace personnel dans la section développeur sur la plateforme de kaliapay.
  
Si vous ne l'avez pas, veuillez-vous connecter. Ensuite, allez dans la section développeur et cliquez sur **Mettre à jour Service**.

## API

#### Explication des termes

`Signin(user, password)`

Cette fonction permet de se connecter à KaliaPay. Elle prend deux paramètres :

`user`: L'identifiant de l'utilisateur (email).

`password`: Le mot de passe de l'utilisateur.

`initialize(amount, custom_data)`

Cette fonction permet d'initialiser un paiement. Elle prend deux paramètres :

`amount`: Le montant du paiement.

`custom_data`: Des données personnalisées associées au paiement.

`getPaymentDetails(reference)`

Cette fonction permet d'obtenir les détails d'un paiement. Elle prend un paramètre :

`reference`: La référence du paiement.

`setTid(value)`

Cette fonction permet de définir le token d'authentification (tid).

`setApiKey(value)`

Cette fonction permet de définir la clé API (apikey).

`setService(value)`

Cette fonction permet de définir l'identifiant du service (service).

**NB : Les paramètres définis dans chaque fonction ci-dessus doivent être renseigner.**

## 2. Configuration
Pour toutes collecte de paiement, vous devez fournir les informations suivantes :

`apikey` : Il s'agit de votre clé d'api. Chaque clé d'api est unique par marchand.

`tid` : Un jeton d'authentification qu'utilise kaliapay pour s'assurer que la demande est légitime. 

`service` : Correspond à l'identifiant du service de collecte.

**NB** : Ce jeton est récupérable à la connexion en effectuant la fonction Signin. Quant à l'apikey en plus de l'exécution de la fonction, il est possible de l'avoir sur la plateforme de kaliapay dans la section développeur où vous y trouverez un tableau dans lequel vous devez copier le contenu de la colonne "Service" et conservez-le.

**Ces informations sont nécessaires pour configurer correctement le module de paiement.**

## Connexion 

Illustration pour éffectuer une connexion et ainsi récupérer votre tid et votre service. Pour ce fait, vous devez utiliser la fonction Signin avec les paramètres suivants : **user**
, **password**. Ci-après l'illustration d'utilisation. 

#### Illustration

```javascript

// Illustration d'utilisation de la fonction Signin pour une connexion

const user = 'test2@gmail.com';
const password = 'xxxxxxxxxx';
Signin(user, password);

```
#### Reponse

```
Réponse de l"API: {
  result: {
    language: 'fr',
    apikey: 'xxxxxxxxxxxxxxxx',
    username: 'xxxxxxxxxxxxx',
    email: 'test2@gmail.com',
    first_name: 'kaliapay ',
    last_name: 'test',
    mfa_enabled: false,
    usergroup: 'users',
    is_team_member: false,
    is_merchant: false,
    tid: 'xxxxxxxxxxxxxxxxxxxxxxxxx'
  },
  state: 'success',
  message: 'Action effectuée avec Succès'
}
```
Vous pouvez maintenant récupérer le tid et l'apikey. Après cela, passons la prochaine étape, celle qui est la collecte de paiement.

## Collecter un Paiement 

Pour éffectuer une collecte de paiement, vous devez utiliser la fonction initialize en lui passant en paramètre des données telles que **amount** et **custom_data**. Ci-après l'illustration d'utilisation.

#### Illustration

```javascript

// Vous devez définir le tid, l'apikey et le service

const tid = 'entrer votre-token-d\'authentification';  
const apikey = 'entrer votre-clé-api';
const service = 'entrer votre-service-id';

kaliapay.setTid(tid);
kaliapay.setApiKey(apikey);
kaliapay.setService(service);

// Exemple d'utilisation de la fonction initialize pour collecter un paiement
const amount = 500;
const custom_data = "entrer votre-custom_data";
kaliapay.initialize(amount, custom_data);

```
#### Reponse

```
Réponse de l"API: {
  result: {
    created: '30/06/2023 - 16:05:47',
    reference: 'd5b1438ed45db573121729eb43c97de77557bed8',
    apikey: 'xxxxxxxxxxxxxxxx',
    service: 'xxxxxxxxxxxxxxxx',
    amount: 500,
    extra: '0700677185',
    target: 'webpay',
    qrcode_url: 'https://paas-qrcode.com/display/ba7fa3094a25f7f31b4e57c339e068beaef90f5a/',
    qrcode_image: 'https://paas-qrcode.com/media/barcodes/30062023160546937924.svg',
    shortened_url: 'https://c4p.me/i181t16547009115',
    description: ''
  },
  state: 'success',
  message: 'Action effectuée avec Succès'
}
```

#### Explication du resultat

`created` : Date de création de la demande de collecte.

`reference` : Référence de la transaction. Vous aurez besoin de cette référence lorsque vous voudrez connaître le statut de la transaction.

`apikey` : C'est votre apikey et celle utilisée pour effectuer cette de demande de collecte.

`service` : L'identifiant du service de collecte de paiement auquel vous êtes abonné.

`amount` : Montant de la transaction.

`extra` : ID de la transaction que vous avez fourni lors de l'initialisation de la transaction.

`target` : Service de collecte interne à kaliapay.

`qrcode_url` : Lien vers un QR Code qui permet à l'utilisateur de scanner et de procéder au paiement.

`qrcode_image` : Image du QR Code. Cette image peut être téléchargée par toute personne qui va scanner le QR Code pour effectuer le paiement.

`shortened_url` : Cette URL redirige vers le guichet de paiement pour procéder à la transaction.


## Détails un Paiement 

Pour prendre connaissance des détails d'un paiement, vous devez utiliser la fonction getPaymentDetails en lui passant la **reference** en paramètre. Ci-après l'illustration d'utilisation avec différentes possibilités de reponses à avoir.

#### Illustration

```javascript

// Vous devez définir le tid, l'apikey et le service

const tid = 'entrer votre-token-d\'authentification';  
const apikey = 'entrer votre-clé-api';
const service = 'entrer votre-service-id';
 
kaliapay.setTid(tid);
kaliapay.setApiKey(apikey);
kaliapay.setService(service);

// Exemple d'utilisation de la fonction getPaymentDetails pour les détails d'un paiement

const reference = 'd5b1438ed45db573121729eb43c97de77557bed8';
kaliapay.getPaymentDetails(reference);

```
#### Reponse 1

```
Réponse de l"API: {
  result: {
    reference: '',
    status: 'TRANSACTION NOT FOUND',
    payment_infos: {}
},
  state: 'success',
  message: 'Action effectuée avec Succès'
}
```

#### Explication de la reponse

Ici dans le bloc **result**, nous avons dans la variable `status: ‘TRANSACTION NOT FOUND’,` ce qui signifie que le système n’a pas encore pris en compte cette référence dans le traitement d’une opération de paiement. Une fois le client redirigé vers la page de paiement, après le scan du qrcode ou la redirection manuelle de celui-ci, le système prend en compte automatiquement la référence si bien évidemment elle existe au préalable.

#### Reponse 2

```
Réponse de l"API: {
  result: {
    reference: '2eacf6b407a217fad781da55570e521238d410ab',
    status: 'NOT INITIATED',
    payment_infos: {}
},
  state: 'success',
  message: 'Action effectuée avec Succès'
}
```

#### Explication de la reponse

Ici le client a ete redirigé vers la page de paiement. Dans le bloc result nous avonc comme `status: ‘NOT INITIATED’`, ce qui signifie que le client est toujours sur la page, à l’étape du choix du canal de paiement. A ce moment les différents canaux de paiement lui sont proposés, il doit sélectionner un canal, fournir les infos et procéder au paiement.

#### Reponse 3

```
Réponse de l"API: {
 "result": {
 "reference": "2eacf6b407a217fad781da55570e521238d410ab",
 "status": "INITIATED",
 "payment_infos": {
 "name": "ORANGE",
 "txnreference": "op4e391681-ecb4-4034-88c5-c005ea7e5dad07062022181013",
 "shortreference": "6KEC841599",
 "external_reference": "b947db90-ffd1-41f4-9bbd-aee9042a23b9",
 "related_qrcode": "",
 "related_qrcode_url": "",
 "express_data": "2eacf6b407a217fad781da55570e521238d410ab",
 "created_at": "07/06/2022 - 18:10:13",
 "htime": "07/06/2022 - 18:10:13",
 "amount": 500,
 "status": "pending",
 "extra_data": "test_custom",
 "apikey": "d0e632be844d55ad5c09b45d6e586fe6a64db6aa",
 "service": "020322171826010677",
 "notif_number": "0709430661",
 "comments": "",
 }
 },
 "state": "success",
 "message": "Action effectuée avec Succès"
}
```

#### Explication de la reponse

Ici le `status: ‘INITIATED’`, ce qui signifie que le client a sélectionné son canal de paiement préféré, dans l’exemple Orange Money, nous constatons que l’opération n’est pas encore terminée. Dans le bloc `payment_infos` nous avons tous les détails de la transactions. Ces infos sont similaires au Callback que nous recevons dans le cas d’un WebPay classique. En cas de **status initialise** à `‘success’` ou `‘successful’` ou encore `‘failed’`, vous pouvez soit délivrer ou non le service. Toutes ces informations vous sont envoyées automatiquement a l’Url de Notification que vous avez spécifié lors de la création du service. Vous pourrez les conserver dans une Base de données pour des raisons d’historique.

### Module

```javascript

// Utilisation du module kaliapay
import * as kaliapay from 'mks-kaliapay';

// Vous devez nécéssairement renseigner ces informations ci-après
const tid = 'entrer votre-token-d\'authentification';  
const apikey = 'entrer votre-clé-api';
const service = 'entrer votre-service-id';

kaliapay.setTid(tid);
kaliapay.setApiKey(apikey);
kaliapay.setService(service);

// Exemple d'utilisation de la fonction Signin pour la connexion

const user = 'entrer votre-email';
const password = 'entrer votre-mot-de-passe';
kaliapay.Signin(user, password);

// Exemple d'utilisation de la fonction initialize pour initaliser un paiement
const amount = 500;
const custom_data = "entrer votre-custom_data";
kaliapay.initialize(amount, custom_data);

// Exemple d'utilisation de la fonction getPaymentDetails pour voir les détails d'un paiement

const reference = 'entrer votre-référence';  // Définissez la référence, vous pourrez l'avoir lors de la requête d'un paiement 
kaliapay.getPaymentDetails(reference);

```





