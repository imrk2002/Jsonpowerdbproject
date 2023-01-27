# Jsonpowerdbproject
# Description -This is project for add data to database and validate the form by using data from database.
# Benifits of JPDB-JsonPowerDB is a Database Server with Developer friendly REST API services. It's a High Performance, Light Weight, Ajax Enabled, Serverless, Simple to Use, Real-time Database. Easy and fast to develop database applications without using any server side programming / scripting or without installing any kind of database.

# Source code
<!DOCTYPE html>
<!--
To change this license header, choose License Headers in Project Properties.
To change this template file, choose Tools | Templates
and open the template in the editor.
-->
<html lang="en">

<head>
    <title>Bootstrap Example</title>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.4.1/css/bootstrap.min.css" />
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
    <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.4.1/js/bootstrap.min.js"></script>
    <script src="http://login2explore.com/jpdb/resources/js/0.0.4/jpdb-commons.js"></script>
    
</head>

<body>
    <div class="container">
        <h2>Vertical (basic) form</h2>
        <form id="empForm" method="post">
            <div class="form-group">
                <span><label for="studrollno">ROll NO:</label>
                    <label id="studMsg"> </label></span>
                <input type="text" class="form-control" name="studrollno" id="studrollno"
                    placeholder="Enter Student Roll No" required />
            </div>
            <div class="form-group">
                <label for="studName">FUll Name:</label>
                <input type="text" class="form-control new" id="studName" placeholder="Enter Student Full Name"
                    name="studName" />
            </div>
            <div class="form-group">
                <label for="studClass">Class:</label>
                <input type="text" class="form-control new" id="studClass" placeholder="Enter Student Class"
                    name="studClass" />
            </div>
            <div class="form-group">
                <label for="birthDate">Birth Date:</label>
                <input type="text" class="form-control new" id="birthDate" placeholder="Enter Student Birth Date"
                    name="birthDate" />
            </div>
            <div class="form-group">
                <label for="studAddress">Address:</label>
                <input type="text" class="form-control new" id="studAddress" placeholder="Enter Student Address"
                    name="studAddress" />
            </div>

            <div class="form-group">
                <label for="enrollmentDate">Enrollment Date:</label>
                <input type="text" class="form-control new" id="enrollmentDate" placeholder="Enter Enrollment Date"
                    name="enrollmentDate" />
            </div>
            <input type="button" class="btn btn-primary" id="empSave" value="Save" onclick="saveEmployee();" />
            <input type="button" class="btn btn-primary" id="empSave" value="Update" onclick="saveEmployee();" />
            <input type="button" class="btn btn-primary" id="empSave" value="Reset" onclick="resetForm();" />
        </form>
    </div>
    <script>
        
    </script>
    <script>
        function validateAndGetFormData() {
            var studrollnoVar = $("#studrollno").val();
            if (studrollnoVar === "") {
                alert("Student Roll No can't be");
                $("#studrollno").focus();
                return "";
            }
            var studNameVar = $("#studName").val();
            if (studNameVar === "") {
                alert("Student name can't be empty");
                $("#studName").focus();
                return "";
            }
            var studClassVar = $("#studClass").val();
            if (studClassVar === "") {
                alert("Student class is required");
                $("#studClass").focus();
                return "";
            }
            var birthDateVar = $("#birthDate").val();
            if (birthDateVar === "") {
                alert("Birth Date is required");
                $("#birthDate").focus();
                return "";
            }
            var studAddressVar = $("#studAddress").val();
            if (studAddressVar === "") {
                alert("Student address is required");
                $("#studAddress").focus();
                return "";
            }
            var enrollmentDateVar = $("#enrollmentDate").val();
            if (enrollmentDateVar === "") {
                alert("Student Enrollment date is required");
                $("#enrollmentDate").focus();
                return "";
            }

            var jsonStrObj = {
                studrollno: studrollnoVar,
                studName: studNameVar,
                studClass: studClassVar,
                birthDate: birthDateVar,
                studAddress: studAddressVar,
                enrollmentDate: enrollmentDateVar

            };
            return JSON.stringify(jsonStrObj);
        }
        // This method is used to create PUT Json request.
        function createPUTRequest(connToken, jsonObj, dbName, relName) {
            var putRequest = "{\n"
                + "\"token\" : \""
                + connToken
                + "\","
                + "\"dbName\": \""
                + dbName
                + "\",\n" + "\"cmd\" : \"PUT\",\n"
                + "\"rel\" : \""
                + relName + "\","
                + "\"jsonStr\": \n"
                + jsonObj
                + "\n"
                + "}";
            return putRequest;
        }
        function executeCommand(reqString, dbBaseUrl, apiEndPointUrl) {
            var url = dbBaseUrl + apiEndPointUrl;
            var jsonObj;
            $.post(url, reqString, function (result) {
                jsonObj = JSON.parse(result);
            }).fail(function (result) {
                var dataJsonObj = result.responseText;
                jsonObj = JSON.parse(dataJsonObj);
            });
            return jsonObj;
        }
        function resetForm() {
            $("#studrollno").val("");
            $("#studName").val("");
            $("#studClass").val("");
            $("#birthDate").val("");
            $("#studAddress").val("");
            $("#enrollmentDate").val("");
            $("#studrollno").focus();
        }
        function saveEmployee() {
            var jsonStr = validateAndGetFormData();
            if (jsonStr === "") {
                return;
            }
            var putReqStr = createPUTRequest("90932265|-31949271087038899|90954006",
                jsonStr, "StudentData", "Std-Reldata");
            alert(putReqStr);
            jQuery.ajaxSetup({ async: false });
            var resultObj = executeCommand(putReqStr,
                "http://api.login2explore.com:5577", "/api/iml");
            alert(JSON.stringify(resultObj));
            jQuery.ajaxSetup({ async: true });
            resetForm();
        }


    </script>
</body>

</html>
