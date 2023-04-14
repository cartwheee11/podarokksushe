<template>
  <router-view/>
</template>



<style>
  html, body {
    padding:  0;
    margin: 0;
  }

  * {
    box-sizing: border-box;
    font-family:  helvetica;
    font-size:  20px;
    line-height: 1.5em;
  }

  input, button {
    height: 50px;
    padding:  0 20px;
    outline:  none;
    border:  none;
    border-radius: 1000px;

  }

  button {
    background-color: #3781f5;
    color:  white;
    position: absolute;
    right: 17px;
    top:  15px;
    background-image: url(assets/send.png);
    background-size: cover;
    height: 50px;
    width: 50px;  
  }

  input {
    background-color: #ebebf3;
    height:  60px;
    padding-right: 70px;
  }

  .clear {
    clear: both;
  }

  a {
    text-transform: none;
    text-decoration: none;
  }

  @media screen and (max-width:  350px) {
    * {
      font-size:  16px;
    }
  }
</style>
