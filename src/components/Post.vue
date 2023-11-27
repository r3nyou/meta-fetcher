<script setup lang="ts">
import { onMounted, resolveComponent } from 'vue';

onMounted(() => {
    FB.getLoginStatus((response) => {
        if (response.status != 'connected') {
            fbLogin()
        } 
        getPostComments(response.authResponse)
    })
})

async function getPostComments(authResponse) {
    try {
        let pages = await getPages(authResponse)
        let post = await getPosts(pages[0])
        let comments = await getComments(pages[0], post[0])
        console.log(pages)
        console.log(post)
        console.log(comments)
    } catch (error) {
        console.error(error)
    }
}

function getPages(authResponse) {
    return new Promise((resolve, reject) => {
        FB.api(
            `/${authResponse.userID}/accounts`,
            'get',
            function (response) {
                if (response && !response.error) {
                    resolve(response.data)
                } else {
                    reject(response.error)
                }
            }
        );
    })
}

function getPosts(page) {
    return new Promise(function (resolve, reject) {
        FB.api(
            `/${page.id}/feed`,
            'get',
            { 'access_token': page.access_token },
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

function getComments(page, post) {
    return new Promise(function (resolve, reject) {
        FB.api(
            `/${post.id}/comments`,
            'get',
            { 'access_token': page.access_token },
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


function fbLogin() {
    FB.login(function(response) {
        if (!response.authResponse) {
            console.log('User cancelled login or did not fully authorize.');
        }
    }, {scope: 'public_profile,pages_show_list,pages_read_engagement,pages_read_user_content'});
}
</script>

<template>
<div>test</div>
</template>
