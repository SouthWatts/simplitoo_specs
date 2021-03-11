Tag Manager Specs for https://www.simplitoo.fr/

# Google Tag Manager initialization

GTM-W7XRFQX container should be called on all pages (same ID for container and staging) : 

- Either in the ```<head>``` + ```<body>``` div of each page :

```html
<!-- Google Tag Manager - head snippet -->
<script>(function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start':
new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0],
j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src=
'https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f);
})(window,document,'script','dataLayer','GTM-W7XRFQX');</script>
<!-- End Google Tag Manager- head snippet  -->
```

```html
<!-- Google Tag Manager - body snippet -->
<noscript><iframe src="https://www.googletagmanager.com/ns.html?id=GTM-W7XRFQX"
height="0" width="0" style="display:none;visibility:hidden"></iframe></noscript>
<!-- End Google Tag Manager - body snippet  -->
```

- Or through a package such as [NPM Google Tag Manager plugin for analytics](https://www.npmjs.com/package/@analytics/google-tag-manager) :

```bash
npm install analytics
npm install @analytics/google-tag-manager
```

```javascript
import Analytics from 'analytics'
import googleTagManager from '@analytics/google-tag-manager'

const analytics = Analytics({
  app: 'awesome-app',
  plugins: [
    googleTagManager({
      containerId: 'GTM-W7XRFQX'
    })
  ]
})
```

Either way, to make sure basic GTM setup is valid, please use [Google Tag Assistant Chrome Plugin](https://chrome.google.com/webstore/detail/tag-assistant-legacy-by-g/kejbdjndbnbjgmefkgdddjlbokphdefk?hl=fr)
(soon to be deprecated). When installed on a page / app where it is set, GTM installation should be validated with a green icon for GTM as following :

<img src="https://i.ibb.co/HY2rm5S/Capture-d-cran-2021-01-22-102033.png" alt="GTM validation" width="250"/>

# Previous tags cleaning

All previous hardcoded Google Analytics / Tag Manager snippets that may exist should be removed from the code, as they will be dealt with through GTM directly, and could cause issues such as double pageviews if they're kept in the codebase.

# Funnel steps tracking

## Société funnel

Each time a Société funnel step is displayed :

```javascript
dataLayer.push({
    'event' : 'societeFunnel',
    'societeFunnelStepName' : 'Votre société', //Step displayed : "Votre société", "Votre activité", "La structure", "Siège social", "Vos statuts", "Nos Formules"...
    'societeFunnelStepNumber' : 2, //Self explanatory
    'societeFunnelCompany' : 'SAS', //Company type : "SAS", "SARL", "EURL"....
    'societeFunnelUserInfos' : { //User infos given by the user on the previous step

        //keys to populate when step "Votre société" has been validated
        'companyNameFilled' : true, //true or false depending if field has been set by user
        'userEmailFilled' : true, //true or false depending if field has been set by user

        //keys to populate when step "Votre Activité" has been validated
        'activityFilled' : true, //true or false depending if field has been set by user

        //keys to populate when step "La structure" has been validated
        'capitalSocial' : 10000, //Value set in "Montant du capital social" in €

        //keys to populate when step "Siège social" has been validated
        'adressType' : 'Chez le gérant', //Value set in "Type d'adresse" : 'Chez le gérant', 'Local commercial', 'Auprès d'une société de domiciliation'

        //keys to populate when step "Nos formules" has been validated
        'formulaName' : 'Basic', //"Basic", "Plus", "Premium" depending on contract type

        //keys to populate when step "Paiement" has been validated
        'transactionAmount' : 332, //Total transaction amount, including administrative fees
        'paymentType' : 'One shot' //"One shot" or "Deferred" (upcoming feature)
    }
})
```

## Association funnel

Each time an Association funnel step is displayed :

```javascript
dataLayer.push({
    'event' : 'assocFunnel',
    'assocFunnelStepName' : 'Votre association', //Step displayed : "Votre association", "Votre activité", "La structure", "Siège social", "Vos statuts", "Nos Formules"...
    'assocFunnelStepNumber' : 2, //Self explanatory
    'assocFunnelUserInfos' : { //User infos given by the user on the previous step

        //keys to populate when step "Votre association" has been validated
        'assocNameFilled' : true, //true or false depending if field has been set by user
        'userEmailFilled' : true, //true or false depending if field has been set by user

        //keys to populate when step "Votre Activité" has been validated
        'activityFilled' : true, //true or false depending if field has been set by user

        //keys to populate when step "Siège social" has been validated
        'adressType' : 'Chez le gérant', //Value set in "Type d'adresse" : 'Chez le gérant', 'Local commercial', 'Auprès d'une société de domiciliation'

        //keys to populate when step "Nos formules" has been validated
        'formulaName' : 'Basic', //"Basic", "Premium" depending on contract type

        //keys to populate when step "Paiement" has been validated
        'transactionAmount' : 332, //Total transaction amount, including administrative fees
        'paymentType' : 'One shot' //"One shot" or "Deferred" (upcoming feature)
    }
})
```

## Protéger ma marque funnel

Each time a given step is loaded :

```javascript
dataLayer.push({
    'event' : 'protectBrandFunnel',
    'protectBrandFunnelStepName' : 'Votre association', //Step displayed : "Votre marque", "Votre activité", "Nos Formules"...
    'protectBrandFunnelStepNumber' : 2, //Self explanatory
    'protectBrandFunnelUserInfos' : { //User infos given by the user on the previous step

        //keys to populate when step "Votre marque" has been validated
        'depotType' : 'Nom de marque', //'Nom de marque' or 'Identité visuelle'

        //keys to populate when step "Votre marque" has been validated
        'protectExtension' : 'Polynesie', //'Polynesie' or 'France métropolitaine'

        //keys to populate when step "Le titulaire" has been validated
        'brandTitulaire' : 'Un particulier', //'Un particulier', 'Une société', 'Une auto-entreprise', 'Une association'

        //keys to populate when step "Nos formules" has been validated
        'formulaName' : 'Basic', //"Basic", "Premium" depending on contract type

        //keys to populate when step "Paiement" has been validated
        'transactionAmount' : 332, //Total transaction amount, including administrative fees
        'paymentType' : 'One shot' //"One shot" or "Deferred" (upcoming feature)
    }
})
```

If a key is not relevant in a given context (ex : 'transactionAmount' before final step), don't include it in the dataLayer.push instruction (no key with no value, key with empty string, key with null or whatever).

To be discussed with product team => See which info is relevant at each step (more complicated than Portail).

# Editorial content loading

Each time an 'actualite' or 'academie' content, **article or category page** is loaded : 

```javascript
dataLayer.push({
    'event' : 'contentLoad',
    'contentType' : 'Article', //"Article" or "Category"
    'contentChars' : 4321, //Number of characters in the article, in the case of 'article' only
    'contentTags' : ['Loi Pacte','SPI'], //List of all tags for an article in a JS array
    'contentAuthor' : 'Anne Perrin', //Self explanatory
    'editoPublishDate' : '2019-08-26' //YYYY-MM-DD date for the article publication date, in the case of 'article' only
})
```

Social share block bottom of 'academie' articles should be decorated with a ```data-tms-bottom-academie``` attribute

!["Social Share"](https://i.ibb.co/DCh3DCv/social.jpg)

```html
<a data-tms-bottom-academie="yes" class="share-action share-icon share-facebook" href="https://www.facebook.com/sharer.php?u=https%3A%2F%2Fstaging.simplitoo.fr%2Factualites%2Fles-changements-apportes-par-la-loi-pacte%2F" original-title="Facebook"><i class="rbi rbi-facebook"></i><span>Share on Facebook</span></a>
```

Previous / next article button at the bottom of 'blog' articles should be decorated with a ```data-tms-bottom-blog``` attribute

!["Next prev"](https://i.ibb.co/fdtNvc8/simplitoo-blog.jpg)
```html
<div class="single-box clearfix" data-tms-bottom-blog="yes">
```

# User login update

Each time there is an update to the login status in the app (login or logout) :

```javascript
dataLayer.push({
    'event' : 'loginStatusUpdate',
    'loginStatusUpdateType' : 'Login', //'Login' or 'Logout'
    'userID' : '123456789' //Simplitoo User ID 
})
```

# Deployment notes

These specs should be first deployed on a staging environment, and only in production in sync with Google Tag Manager publication managed by SouthWatts.

# Contact

Please reach aristide.riou@southwatts.com for any questions