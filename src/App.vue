<template></template>

<script>
// TODO env
// TODO separate component
// TODO store token in memory
// TODO store token in firebase
const facebookAppId = 246637738109326
const pageId = 171613189363559
const access_token = 'EAADgUNLEqY4BO8pgbZA1wyDpqE5niBTZBrEmGifrQECBmszpI9EvOw9KZCKZA1hDvYyDwwiAsp0NxzSgZBZBWiG3K7ZCdEZCAHZBwh9QkWY4dITBWhINhRZCTpl3N5innunPnAm1TJijMcBZAPkJOctL5BD466auNxxXiNTDbvqob4Wk7sdYOijMW0f9V21gwaYkQbO6TwufeUXcOU8TLCOwStzbfMineyZCuxVwrK7nKOLp'

const FB = await getFbSdk()

FB.api(
  `/${pageId}/feed`,
  'get',
  { 'access_token': access_token },
  function (response) {
    if (response && !response.error) {
      console.log('posts', response)
    } else {
      console.log('error', response.error)
    }
  }
);

const postId = '171613189363559_122107944680099772'
FB.api(
  `/${postId}/comments`,
  'get',
  { 'access_token': access_token },
  function (response) {
    if (response && !response.error) {
      console.log('comments', response)
    } else {
      console.log('error', response.error)
    }
  }
)

function initFacebookSdk() {
  return new Promise(resolve => {
    // wait for facebook sdk to initialize before starting the vue app
    window.fbAsyncInit = function () {
      const FB = window.FB;
      FB.init({
        appId: facebookAppId,
        cookie: true,
        xfbml: true,
        version: 'v18.0'
      });
      resolve();
    };

    // load facebook sdk script
    (function (d, s, id) {
      var js, fjs = d.getElementsByTagName(s)[0];
      if (d.getElementById(id)) { return; }
      js = d.createElement(s); js.id = id;
      js.src = "https://connect.facebook.net/en_US/sdk.js";
      fjs.parentNode.insertBefore(js, fjs);
    }(document, 'script', 'facebook-jssdk'));
  });
}

function getFbSdk() {
  return new Promise(async resolve => {
    if (window.FB) {
      resolve(window.FB)
    } else {
      await initFacebookSdk()
      resolve(window.FB)
    }
  })
}
</script>