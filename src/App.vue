<template></template>

<script>
// TODO fb login get user_id, token
// TODO get page token
// TODO env
// TODO separate component
// TODO handle page paginate
// TODO store token in firebase
const facebookAppId = 246637738109326
const pageId = 171613189363559
const access_token = ''

const FB = await getFbSdk()
getFbPageComments(pageId)

async function getFbPageComments(pageId) {
  try {
    let posts = await getFbPosts(pageId)

    let res = []
    for (let post of posts) {
      let comments = await getFbComments(post.id)
      post['comments'] = comments.map(a => a.message)
      res.push(post)
    }

    console.log(res)
  } catch (error) {
    console.error(error)
  }
}

function getFbPosts(pageId) {
  return new Promise(function (resolve, reject) {
    FB.api(
      `/${pageId}/feed`,
      'get',
      { 'access_token': access_token },
      function (response) {
        if (response && !response.error) {
          resolve(response.data)
        } else {
          reject(response.error)
        }
      }
    )
  });
}

function getFbComments(postId) {
  return new Promise(function (resolve, reject) {
    FB.api(
      `/${postId}/comments`,
      'get',
      { 'access_token': access_token },
      function (response) {
        if (response && !response.error) {
          resolve(response.data)
        } else {
          reject(response.error)
        }
      }
    )
  })
}

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