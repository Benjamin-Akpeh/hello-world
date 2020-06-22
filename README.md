<html>
<head>
<title>Pandora Transportation</title>
<?php include("header.php");?>
</head>
<body>
<?php include("C:\MyCodes/pandoratransportation2/extra/headers.php");?>


<div class="" style="margin-top:1%; padding-top:3px;">
  <img src="images\bus2.jpg" alt="Background" style="width:100%; height:80%; margin-top: 3%; margin-bottom: 3%;">
</div>


<div class="container"> 
  <div class="" style="margin-top:5%;">
    <h3>BOOK A TRIP</h3>
    <form action="" method="POST" >
      <div class="row">
        <div class="col-md-6">
            <div class="label" >Departing From</div>
            <select id="sel_location" class="form-control" name="location">
              <option value="">- Select Departure -</option>
              <?php 
              // Fetch Department
              $sql_location = "SELECT * FROM location";
              $location_data = mysqli_query($con,$sql_location);
              while($row = mysqli_fetch_assoc($location_data) ){
                  $locationid = $row['lc_id'];
                  $location_name = $row['lc_name'];
                  
                  // Option
                  echo "<option value='".$locationid."' >".$location_name."</option>";
              }
              ?>
            </select>

            <div class="label" >Destination</div>
            <select name="Destination" id="sel_destination" class="form-control">
              <option value="">Select Destination</option>
            </select>
            <div class="label" >Available Buses</div>
            <select name="Buses" id="sel_bus" class="form-control">
              <option value="">Select bus</option>
            </select>

            <div class="label">Journey Date</div>
            <input type="date" class="form-control" name="Journeydate" id="journeydate">            
        </div>
        <div class="col-md-6" style="">
            <div class="label">Return Date</div>
            <input type="date" class="form-control" name="returnDate" id="returndate">
            <div class="label">Mobile No</div>
            <input type="number" required="required" class="form-control" name="mobileno" id="mobilenum">
            <div class="label">Name</div>
            <input type="text" class="form-control" name="name" id="name" />
            
            <div class="label">Email</div>
            <input type="email" class="form-control" name="email" id="email">
            
            <div class="label">No of Tickets</div>
            <input type="number" class="form-control" name="nooftickets" id="ticket">
            <br/>
              
              <button type="button" class="btn btn-primary btn" onclick="openModal()" data-toggle="modal" data-target="#myModal">Preview</button>
              
    </form>

    <div class="modal fade" id="myModal" tabindex="-1" role="dialog" aria-labelledby="myModalLabel" aria-hidden="true">
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <button type="button" class="close" data-dismiss="modal" aria-hidden="true">&times;</button>
            <h4 class="modal-title" id="myModalLabel">Booking Details</h4>
          </div>
          <form action="" method="POST">
            <div class="modal-body">
            Departing from: <b id="departingfromfield"></b>
            <input type="hidden" name="prdepartingfrom" id="departingfrominput"><br/>
            Destination: <b id="destinationfield"></b>
            <input type="hidden" name="prdestination" id="destinationinput"><br/>
            Available buses: <b id="availablebusfield"></b>
            <input type="hidden" name="pravailablebus" id="availablebusinput"><br/>
            Journey date: <b id="journeydatefield"></b>
            <input type="hidden" name="prjourneydate" id="journeydateinput"><br/>
            Return date: <b id="returndatefield"></b>
            <input type="hidden" name="prreturndate" id="returndateinput"><br/>
            Mobile no: <b id="mobilenofield"></b>
            <input type="hidden" name="prmobileno" id="mobilenoinput"><br/>
              Name: <b id="namefield"></b>
              <input type="hidden" name="prname" id="nameinput"><br/>
              Email: <b id="emailfield"></b>
              <input type="hidden" name="premail" id="emailinput"><br/>
              No of tickets: <b id="ticketfield"></b>
              <input type="hidden" name="prticket" id="ticketinput"><br/>
            </div>
            <div class="modal-footer">
              <button type="button" class="btn btn-default" data-dismiss="modal">Close</button>
              <button type="submit" value="Book Now" class="btn btn-primary btn-sm" name="book">Book</button>
            </div>
          </form>
        </div> 
      </div>
    </div>
    
        
<script>
$(document).ready(function(){
  $("#sel_location").change(function(){
      var locid = $(this).val();
      $.ajax({
          url: '?page=getDestination',
          type: 'post',
          data: {location:locid},
          dataType: 'json',
          success:function(response){
            var len = response.length;
            $("#sel_destination").empty();
            for( var i = 0; i<len; i++){
                var ds_lc_id = response[i]['ds_lc_id'];
                var ds_name = response[i]['ds_name'];
                $("#sel_destination").append("<option value='"+ds_lc_id+"'>"+ds_name+"</option>");
            }
          }
      });
  });
});
</script>
<script>

$(document).ready(function(){

$("#sel_destination").change(function(){
    var desid = $(this).val();

    $.ajax({
        url: '?page=getBuses',
        type: 'post',
        data: {destination:desid},
        dataType: 'json',
        success:function(response){

            var len = response.length;

            $("#sel_bus").empty();
            for( var i = 0; i<len; i++){
                var bs_ds_id = response[i]['bs_ds_id'];
                var bs_name = response[i]['bs_name'];
                
                $("#sel_bus").append("<option value='"+bs_ds_id+"'>"+bs_name+"</option>");

            }
        }
    });
});

});

function openModal(){
  
  document.getElementById("namefield").innerHTML = document.getElementById("name").value;
  document.getElementById("nameinput").value =document.getElementById("name").value;
  document.getElementById("emailfield").innerHTML = document.getElementById("email").value;
  document.getElementById("emailinput").value =document.getElementById("email").value;
   document.getElementById("ticketfield").innerHTML = document.getElementById("ticket").value;
   document.getElementById("ticketinput").value =document.getElementById("ticket").value;
   document.getElementById("returndatefield").innerHTML = document.getElementById("returndate").value;
   document.getElementById("returndateinput").value =document.getElementById("returndate").value;
   document.getElementById("mobilenofield").innerHTML = document.getElementById("mobilenum").value;
   document.getElementById("mobilenoinput").value =document.getElementById("mobilenum").value;
   document.getElementById("journeydatefield").innerHTML = document.getElementById("journeydate").value;
   document.getElementById("journeydateinput").value =document.getElementById("journeydate").value;
   var e = document.getElementById("sel_location");
   var selResult = e.options[e.selectedIndex].text
   document.getElementById("departingfromfield").innerHTML = selResult;
   document.getElementById("departingfrominput").value =document.getElementById("sel_location").value;
   var e = document.getElementById("sel_destination");
   var selResult = e.options[e.selectedIndex].text
   document.getElementById("destinationfield").innerHTML = selResult;
   document.getElementById("destinationinput").value =document.getElementById("sel_destination").value;
   var e = document.getElementById("sel_bus");
   var selResult = e.options[e.selectedIndex].text
   document.getElementById("availablebusfield").innerHTML = selResult;
   document.getElementById("availablebusinput").value =document.getElementById("sel_bus").value;

  $("#myModal").modal('show');
}
</script>


</body>
</html>

