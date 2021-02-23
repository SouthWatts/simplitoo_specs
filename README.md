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

# Funnel steps load

Each time a given step is loaded :

```javascript
dataLayer.push({
    'event' : 'funnelStep',
    'funnelName' : 'Votre marque',//Name of the funnel : "Créer ma société", "Créer mon association", "Protéger ma marque"...
    'funnelStepInfo': {
        'stepName' : 'Vos statuts', //Step name, as displayed on the site
        'stepNumber' : 2, //Self explanatory
        'companyType' : 'EURL', //Company type : EURL, SARL...
        'transactionAmount' : 59 //Only in the last step, transaction amount
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
    'contentType' : 'article', //'article' or 'category'
    'contentChars' : 4321, //Number of characters in the article, in the case of 'article' only
    'contentTags' : ['Loi Pacte','SPI'], //List of all tags for an article in a JS array
    'contentAuthor' : 'Anne Perrin', //Self explanatory
    'editoPublishDate' : '2019-08-26' //YYYY-MM-DD date for the article publication date, in the case of 'article' only
})
```

Social share block bottom of 'actualite' or 'academie' articles should be decorated with a ```data-tms-social-bottom``` attribute

!["Social Share"](https://i.ibb.co/DCh3DCv/social.jpg)

```html
<a data-tms-social-bottom="yes" class="share-action share-icon share-facebook" href="https://www.facebook.com/sharer.php?u=https%3A%2F%2Fstaging.simplitoo.fr%2Factualites%2Fles-changements-apportes-par-la-loi-pacte%2F" original-title="Facebook"><i class="rbi rbi-facebook"></i><span>Share on Facebook</span></a>
```

# User login update

Each time there is an update to the login status in the app (login or logout) :

```javascript
dataLayer.push({
    'event' : 'loginStatusUpdate',
    'loginStatusUpdateType' : 'login', //'login' or 'logout'
    'userID' : '123456789' //Simplitoo User ID 
})
```

# Chatbot tracking

See if possible to get some data from landbot and do a postMessage or parent.dataLayer.push. To be discussed.

# Deployment notes

These specs should be first deployed on a staging environment, and only in production in sync with Google Tag Manager publication managed by SouthWatts.

# Contact

Please reach aristide.riou@southwatts.com for any questions