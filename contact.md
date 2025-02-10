---
layout: page
title: "Contact Me"
description: "Contact Author"
---
<style>
    .contact-form {
        width: 80%;
        margin: 0 auto;
        padding: 20px;
        border: 1px solid #eee;
        border-radius: 5px;
        box-shadow: 0 2px 3px #ccc;
        background-color: #f9f9f9;
        font-family: Arial, sans-serif;
    }
    .form-group {
        margin-bottom: 20px;
    }
    .form-group label {
        margin-bottom: 5px;
        font-weight: bold;
    }
    .form-control {
        width: 98%;
        padding: 1%;
        border: 1px solid #ccc;
        border-radius: 3px;
    }
    .btn {
        display: block;
        width: 100%;
        padding: 10px;
        background-color: #C73B45;
        color: white;
        border: none;
        border-radius: 3px;
        cursor: pointer;
        transition: all 0.3s ease-in-out;
    }
    .btn:hover {
        background-color: #A33A41;
        box-shadow: 0 2px 5px rgba(0,0,0,0.1);
    }
</style>

<form
  action="https://formspree.io/f/mkgozlqk"
  method="POST"
  class="contact-form"
>
    <div class="form-group">
        <label for="name">Name:</label>
        <input type="text" name="name" id="name" class="form-control" required>
    </div>
    <div class="form-group">
        <label for="email">Email Address:</label>
        <input type="email" name="email" id="email" class="form-control" required>
    </div>
    <div class="form-group">
        <label for="message">Message:</label>
        <textarea name="message" id="message" class="form-control" rows="5" required></textarea>
    </div>
    <button type="submit" class="btn btn-primary">Send</button>
</form>
