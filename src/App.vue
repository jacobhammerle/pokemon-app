<template>
    <div id="app">
        <nav>
            <router-link to="/">Home</router-link>
            <router-link to="/search">Search</router-link>
        </nav>
        <div id="body">
            <router-view/>
        </div>
    </div>
</template>
