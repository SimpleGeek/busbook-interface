<style>
    /* Begin styling for the nav */
    /* Add a black background color to the top navigation */
    .topnav {
        background-color: DodgerBlue;
        overflow: hidden;
    }

    /* Style the links inside the navigation bar */
    .topnav a {
        float: left;
        display: block;
        color: #f2f2f2;
        text-align: center;
        padding: 14px 16px;
        text-decoration: none;
        font-size: 17px;
    }

    /* Change the color of links on hover */
    .topnav a:hover {
        background-color: rgba(250, 250, 250);
        color: black;
    }

    .link-txt {
        display: inline;
    }

    @media screen and (max-width: 500px) {
        .link-txt {
            display: none;
        }
    }
    /* End styling for the nav */

    /* Begin styling for sign out modal */
    * {box-sizing: border-box}

    /* Set a style for all buttons */
    button {
        background-color: #4CAF50;
        color: white;
        padding: 14px 20px;
        margin: 8px 0;
        border: none;
        cursor: pointer;
        width: 100%;
        opacity: 0.9;
    }

    button:hover {
        opacity:1;
    }

    /* Float cancel and delete buttons and add an equal width */
    .cancelbtn, .deletebtn {
        float: left;
        width: 50%;
    }

    /* Add a color to the cancel button */
    .cancelbtn {
        background-color: #ccc;
        color: black;
    }

    /* Add a color to the delete button */
    .deletebtn {
        background-color: #f44336;
    }

    /* Add padding and center-align text to the container */
    .container {
        padding: 16px;
        text-align: center;
    }

    /* The Modal (background) */
    .modal {
        display: none; /* Hidden by default */
        position: fixed; /* Stay in place */
        z-index: 1; /* Sit on top */
        left: 0;
        top: 0;
        width: 100%; /* Full width */
        height: 100%; /* Full height */
        overflow: auto; /* Enable scroll if needed */
        background-color: #474e5d;
        padding-top: 50px;
    }

    /* Modal Content/Box */
    .modal-content {
        background-color: #fefefe;
        margin: 5% auto 15% auto; /* 5% from the top, 15% from the bottom and centered */
        border: 1px solid #888;
        width: 80%; /* Could be more or less, depending on screen size */
    }

    /* The Modal Close Button (x) */
    .close {
        position: absolute;
        right: 35px;
        top: 15px;
        font-size: 40px;
        font-weight: bold;
        color: #f1f1f1;
    }

    .close:hover,
    .close:focus {
        color: #f44336;
        cursor: pointer;
    }

    /* Clear floats */
    .clearfix::after {
        content: "";
        clear: both;
        display: table;
    }

    /* Change styles for cancel button and delete button on extra small screens */
    @media screen and (max-width: 300px) {
        .cancelbtn, .deletebtn {
            width: 100%;
        }
    }
    /* End styling for sign out modal */
</style>

<script>
    function hideSignOutModal() {
        document.getElementById('modal').style.display = 'none';
    }

    function showSignOutModal() {
        document.getElementById('modal').style.display = 'block';
    }
</script>

<div class="topnav">
    <a href="#a"><i class="fas fa-home"></i> <div class="link-txt">Home</div></a>
    <a href="#a"><i class="fas fa-info"></i> <div class="link-txt">About</div></a>
    <a href="#a"><i class="fas fa-question"></i> <div class="link-txt">Help</div></a>
    <a href="#a" style="float: right" on:click="{showSignOutModal}">
        <i class="fas fa-door-open"></i> <div class="link-txt">Sign Out</div>
    </a>
</div>

<!-- Begin sign out modal -->
<div id="modal" class="modal">
    <span on:click={hideSignOutModal} class="close" title="Close Modal">&times;</span>
    <form class="modal-content">
      <div class="container">
        <h1>Sign Out</h1>
        <p>Are you sure you want to sign out of your account?</p>
  
        <div class="clearfix">
          <button type="button" class="cancelbtn" on:click={hideSignOutModal}>Cancel</button>
          <button type="button" class="deletebtn">Sign Out</button>
        </div>
      </div>
    </form>
</div>
<!-- End sign out modal -->