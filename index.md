<!DOCTYPE html>
<html lang="en">
<head>
  <title>Bootstrap Example</title>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.4.1/css/bootstrap.min.css">
  <!-- <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>  -->
  <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.4.1/js/bootstrap.min.js"></script>

  <link rel="stylesheet" href="style.css">
</head>

<body onload="listMsgs()">
  <!-- The Modal -->
  <div id="myModal" class="modal">

    <!-- Modal content -->
    <div class="modal-content">
      <span class="close">&times;</span>
      <p>Invalid message length :(</p>
    </div>

  </div>

  
  <div class="container">

    <div class="row">
      <div class="col-lg-4 col-lg-offset-4">
          <div class="input-group">
              <input type="text" class="form-control" id="postText" /> 
              <span class="input-group-btn">
                  <button class="btn btn-default" type="button" onclick="addMsg()">Go!</button>
              </span>
          </div>
          <div id="messages"> </div> 
      </div>
    </div>     
  </div>




  </body>
</html>


<script>

function tooLongMessage()
{
  var modal = document.getElementById("myModal");
  modal.style.display = "block";

  var span = document.getElementsByClassName("close")[0];

  // When the user clicks on <span> (x), close the modal
  span.onclick = function() {
    modal.style.display = "none";
  }
}

// Vet inte om denna behövs, tänkte användas till checkbox toggle på något sätt.
function checkboxToggle(id)
{
  document.getElementById(id).checked = !document.getElementById(id).checked;
}

// Changed the msgbox color depending on change of checkbox.
function markRead(dateID)
{
  let msg = document.getElementById(dateID);
  if (msg.childNodes[0].checked)
  {
    msg.setAttribute("class", "readMsgBox");
    // Blir knas i utskriften
    //document.cookie = `${msg.id}=${msg.innerHTML}`;
  }
  else
  {
    msg.setAttribute("class", "msgBox");
    //document.cookie = `${msg.id}=${msg.innerHTML}`;
  }
}

function createMessage(keyValue)
{
  // Lägger in meddelanden på rad.
  let date_id = keyValue.split('=')[0];
  let message = keyValue.split('=')[1];
  let msg = document.createElement('div');
  msg.setAttribute("class", "msgBox");
  msg.setAttribute("checked", false);
  msg.setAttribute("id", date_id);
  msg.innerHTML = `<p>${message}</p>`;
  
  checkbox = document.createElement('input');
  checkbox.type = 'checkbox';
  checkbox.setAttribute( "onchange", `markRead(${msg.id});`); // Adds a function dynamically to the checkbox.
  // Puts checkbox before the text in the msgbox.
  msg.insertBefore(checkbox, msg.firstChild);

  // Makes sure last message is seen at the top of msgs.
  let messages = document.getElementById('messages');
  messages.insertBefore(msg, messages.firstChild);
}

function addMsg()
{
  let msg = document.getElementById('postText').value;
  if (msg.length == 0 || msg.length > 140)
  {
    tooLongMessage();
  }
  else
  {
    keyValue = `${Date.now()}=${msg}`;
    document.cookie = keyValue;

    createMessage(keyValue);
  }
}

function listMsgs()
{  
  //alert(document.cookie);
  
  if (document.cookie != "")
  {
    let keyValues = document.cookie.split(";");
    
    keyValues.forEach(keyValue => createMessage(keyValue));
    
  }
}


</script> 
