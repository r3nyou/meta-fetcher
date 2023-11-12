<template></template>

<script>
// TODO env
// TODO separate component
// TODO store token in memory
// TODO store token in firebase
const facebookAppId = 246637738109326
const pageId = 171613189363559

const FB = await getFbSdk()
FB.api(
  `/${pageId}/feed`,
  'get',
  { 'access_token': 'EAADgUNLEqY4BOZBfZBZA9WmUrr8geY3ZBBQ6zhBAsdO0PV18auyYaZBEl8d77hDw6MmRqboKNJH8TzatUXYeJ19HU785JnoxqRpyC6n9tDkzHNJLt4naPpeeECbQDPyzMab9ZBWgyWa6Y03GpE53vOHTAwqJxCSKvG1eWIetewwe9WRxSJZBq0olY3h9iP5qqkHfpyNoSbc1ZC7sClp9719RKSLHyULXnEdOKFB6DvIZD' },
  function (response) {
    if (response && !response.error) {
      console.log(response)
    } else {
      console.log('error', response.error)
    }
  }
);

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