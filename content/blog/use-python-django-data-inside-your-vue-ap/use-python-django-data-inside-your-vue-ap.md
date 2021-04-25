---
title: Use Python & Django Data Inside Your Vue App
date: 2021-03-11T20:28:39.218Z
description: Two approaches to passing data
---



](/use-python-django-data-inside-your-vue-app-9565637b3b95?source=post_page-----9565637b3b95--------------------------------)

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9565637b3b95&operation=register&redirect=https%3A%2F%2Fbennettgarner.medium.com%2Fuse-python-django-data-inside-your-vue-app-9565637b3b95&source=post_actions_header--------------------------bookmark_preview-----------)

![](https://miro.medium.com/max/600/1*lgunwCjBIZvosxo5W3HHQg.png?q=20)



I recently got a good [question from a reader](https://medium.com/@diegogaona/hi-bennet-thanks-for-your-great-articles-a66813b5e617):

> Using Vue CLI, how we can use Django data in Vue? Suppose I have a pandas dataframe or dictionary I want to pass to the Vue app, how to do it?

# Two Approaches to Passing Data

If you’re using [Vue with Django](https://medium.com/@diegogaona/hi-bennet-thanks-for-your-great-articles-a66813b5e617) as I’ve described, there are two ways to pass data to your components:

1\. [Expose an API for your data](https://medium.com/swlh/build-your-first-rest-api-with-django-rest-framework-e394e39a482c) and use `fetch` or `axios` inside your Vue component. (Usually inside the `mounted()` method)

2\. Pass the data in via the Django view/template and then set it as a prop on your vue component. For instance

def my\_view(request):  
    data = build\_your\_data()  
    return render(request, ‘test.html’, {‘data’: data}

Then in `test.html`:

<div id=”app”>  
  <my-component data={{ data }}>  
</div>

And your Vue component:

<template>  
  {{ data }}  
</template>  
<script>  
module.exports = {  
  props: \[‘data’\]  
}  
</script>