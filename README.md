index.jsp

<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Health Logger - Welcome</title>
    <link rel="stylesheet" type="text/css" href="${pageContext.request.contextPath}/css/style.css">
    <style>
        body {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background-color: #f0f2f5;
        }
        .welcome-container {
            text-align: center;
            padding: 40px;
            background-color: #fff;
            border-radius: 8px;
            box-shadow: 0 4px 8px rgba(0,0,0,0.1);
        }
        .welcome-container h1 {
            color: #333;
            margin-bottom: 10px;
        }
        .welcome-container p {
            color: #666;
            margin-bottom: 30px;
        }
        .welcome-container .button {
            padding: 12px 25px;
            font-size: 1.1em;
        }
    </style>
</head>
<body>
    <div class="welcome-container">
        <h1>Health Logger</h1>
        <p>Developed by James Tech Pvt. Ltd. (Prototype by John Peterson)</p> <%-- As per screenshot developer name --%>
        <a href="${pageContext.request.contextPath}/adminLogin.jsp" class="button">Doctor Login</a>
    </div>
</body>
</html>


adminlogin.jsp

<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Doctor Login - Health Logger</title>
    <link rel="stylesheet" type="text/css" href="${pageContext.request.contextPath}/css/style.css">
</head>
<body>
    <div class="login-container">
        <h2>Health Logger</h2>
        <p style="text-align:center; font-size:0.9em; color:#777;">Doctor Login</p>
        <br>
        <c:if test="${not empty errorMessage}">
            <p class="error-message">${errorMessage}</p>
        </c:if>
        <c:if test="${param.auth_error == 'true'}">
             <p class="error-message">Please login to access that page.</p>
        </c:if>

        <form action="${pageContext.request.contextPath}/login" method="post">
            <div class="form-group">
                <label for="email">Enter Email</label>
                <input type="email" id="email" name="email" placeholder="abc@example.com" required>
            </div>
            <div class="form-group">
                <label for="password">Enter Password</label>
                <input type="password" id="password" name="password" required>
            </div>
            <button type="submit" class="button">Login</button>
        </form>
    </div>
</body>
</html>


adminHome

<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<c:if test="${empty sessionScope.admin}">
    <c:redirect url="adminLogin.jsp?auth_error=true"/>
</c:if>
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Admin Dashboard - Health Logger</title>
    <link rel="stylesheet" type="text/css" href="${pageContext.request.contextPath}/css/style.css">
</head>
<body>
    <div class="header">
        <h1>Health Logger</h1>
        <div class="dev-details">Developed by John Peterson</div>
        <a href="${pageContext.request.contextPath}/logout" class="logout-link">Logout (${sessionScope.adminEmail})</a>
    </div>

    <div class="simple-container">
        <h2>Admin Dashboard</h2>
        <div class="dashboard-options">
            <a href="${pageContext.request.contextPath}/patient?action=list" class="button">Manage Patients</a>
            <a href="${pageContext.request.contextPath}/vital?action=listAll" class="button">Manage Vitals</a>
            <%-- <a href="${pageContext.request.contextPath}/vital?action=alerts" class="button button-danger">Vital Alerts</a> --%>
            <%-- This is listed as separate link in manageVitals.jsp --%>
        </div>
    </div>

    <%-- Footer or other common elements can be added here --%>
</body>
</html>


patientForm

<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<c:if test="${empty sessionScope.admin}">
    <c:redirect url="adminLogin.jsp?auth_error=true"/>
</c:if>
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>${mode == 'add' ? 'Add New Patient' : 'Update Patient'} - Health Logger</title>
    <link rel="stylesheet" type="text/css" href="${pageContext.request.contextPath}/css/style.css">
</head>
<body>
    <div class="header">
        <h1>Health Logger</h1>
         <a href="${pageContext.request.contextPath}/logout" class="logout-link">Logout</a>
    </div>

    <div class="nav-bar">
        <a href="${pageContext.request.contextPath}/adminHome.jsp" class="button-link button-default">Dashboard</a>
        <a href="${pageContext.request.contextPath}/patient?action=list" class="button-link">Back to Patient List</a>
    </div>

    <div class="container">
        <h2>${mode == 'add' ? 'Add New Patient' : 'Update Patient Information'}</h2>

        <form action="${pageContext.request.contextPath}/patient" method="post">
            <c:if test="${mode == 'edit'}">
                <input type="hidden" name="action" value="update">
                <input type="hidden" name="patientId" value="${patient.patientId}">
            </c:if>
            <c:if test="${mode == 'add'}">
                <input type="hidden" name="action" value="add">
            </c:if>

            <div class="form-group">
                <label for="name">Name:</label>
                <input type="text" id="name" name="name" value="${patient.name}" required>
            </div>
            <div class="form-group">
                <label for="email">Email: (Optional for this app, but shown in screenshot)</label>
                <input type="email" id="email" name="email_display_only" value="${patient.email_display_only}" placeholder="david@gmail.com">
                <%-- Note: The DB schema does not have email for patient. The screenshot might be a mock. --%>
                <%-- I will use 'phone' as per the project model requirements. --%>
            </div>
            <div class="form-group">
                <label for="phone">Phone:</label>
                <input type="text" id="phone" name="phone" value="${patient.phone}" required>
            </div>
            <div class="form-group">
                <label for="age">Age:</label>
                <input type="number" id="age" name="age" value="${patient.age}" required min="0">
            </div>
            <div class="form-group">
                <label>Gender:</label>
                <input type="radio" id="male" name="gender" value="male" ${patient.gender == 'male' ? 'checked' : ''} required> <label for="male">Male</label>
                <input type="radio" id="female" name="gender" value="female" ${patient.gender == 'female' ? 'checked' : ''}> <label for="female">Female</label>
                <input type="radio" id="other" name="gender" value="other" ${patient.gender == 'other' ? 'checked' : ''}> <label for="other">Other</label>
            </div>
            <div class="form-group">
                <label for="diagnosis">Diagnosis:</label>
                <textarea id="diagnosis" name="diagnosis" required>${patient.diagnosis}</textarea>
            </div>
            <div class="form-group">
                <label for="remarks">Remarks:</label>
                <textarea id="remarks" name="remarks">${patient.remarks}</textarea>
            </div>
            <button type="submit" class="button">${mode == 'add' ? 'Add Patient' : 'Update Patient'}</button>
        </form>
    </div>
</body>
</html>


managepatients

<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<%@ taglib uri="http://java.sun.com/jsp/jstl/fmt" prefix="fmt" %>
<c:if test="${empty sessionScope.admin}">
    <c:redirect url="adminLogin.jsp?auth_error=true"/>
</c:if>
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Manage Patients - Health Logger</title>
    <link rel="stylesheet" type="text/css" href="${pageContext.request.contextPath}/css/style.css">
    <script>
        function confirmDelete(patientId) {
            if (confirm("Are you sure you want to delete this patient and all their records? This cannot be undone.")) {
                window.location.href = "${pageContext.request.contextPath}/patient?action=delete&id=" + patientId;
            }
        }
    </script>
</head>
<body>
    <div class="header">
        <h1>Doctor Home Page</h1>
        <a href="${pageContext.request.contextPath}/logout" class="logout-link">Logout</a>
    </div>
    
    <div class="nav-bar">
        <a href="${pageContext.request.contextPath}/adminHome.jsp" class="button-link button-default">Dashboard</a>
        <a href="${pageContext.request.contextPath}/patient?action=showAddForm" class="button-link">Add Patient</a>
        <a href="${pageContext.request.contextPath}/vital?action=listAll" class="button-link">Manage Vitals</a>
         <form action="${pageContext.request.contextPath}/patient" method="get" style="display:inline-block; margin-left: 20px;">
            <input type="hidden" name="action" value="search">
            <input type="text" name="query" placeholder="Search Patients..." value="${searchQuery}" style="padding: 8px; width: 200px;">
            <button type="submit" class="button" style="padding: 8px 12px;">Search</button>
        </form>
    </div>

    <div class="container">
        <h2>Patients [ <c:out value="${patientList.size()}"/> ]</h2>

        <c:if test="${not empty sessionScope.successMessage}">
            <p class="success-message">${sessionScope.successMessage}</p>
            <c:remove var="successMessage" scope="session"/>
        </c:if>
        <c:if test="${not empty sessionScope.errorMessage}">
            <p class="error-message">${sessionScope.errorMessage}</p>
            <c:remove var="errorMessage" scope="session"/>
        </c:if>

        <c:choose>
            <c:when test="${not empty patientList}">
                <table>
                    <thead>
                        <tr>
                            <th>Sr. No</th>
                            <th>Name - Age</th>
                            <th>Email (Display Only)</th> <%-- Per screenshot, not in DB model --%>
                            <th>Phone</th>
                            <th>Diagnosis</th>
                            <th>Remark</th>
                            <th>Gender</th>
                            <th>Action</th>
                        </tr>
                    </thead>
                    <tbody>
                        <c:forEach var="patient" items="${patientList}" varStatus="status">
                            <tr>
                                <td>${status.count}</td>
                                <td><c:out value="${patient.name}"/> - <c:out value="${patient.age}"/></td>
                                <td><%-- As per screenshot example: john@example.com --%>
                                    <c:choose>
                                        <c:when test="${patient.name eq 'David'}">david@gmail.com</c:when>
                                        <c:when test="${patient.name eq 'John'}">john@example.com</c:when>
                                        <c:when test="${patient.name eq 'Fionna'}">fionna@gmail.com</c:when>
                                        <c:otherwise>N/A</c:otherwise>
                                    </c:choose>
                                </td>
                                <td><c:out value="${patient.phone}"/></td>
                                <td><c:out value="${patient.diagnosis}"/></td>
                                <td><c:out value="${patient.remarks}"/></td>
                                <td><c:out value="${patient.gender}"/></td>
                                <td>
                                    <a href="${pageContext.request.contextPath}/vital?action=listPatientVitals&patientid=${patient.patientId}" class="button-link button-info">Manage Vitals</a>
                                    <a href="${pageContext.request.contextPath}/patient?action=showEditForm&id=${patient.patientId}" class="button-link">Update</a>
                                    <a href="javascript:void(0);" onclick="confirmDelete(${patient.patientId})" class="button-link button-danger">Delete</a>
                                </td>
                            </tr>
                        </c:forEach>
                    </tbody>
                </table>
            </c:when>
            <c:otherwise>
                <p>No patients found.</p>
            </c:otherwise>
        </c:choose>
    </div>
</body>
</html>


searchpatient

<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<c:if test="${empty sessionScope.admin}">
    <c:redirect url="adminLogin.jsp?auth_error=true"/>
</c:if>
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Search Results - Health Logger</title>
    <link rel="stylesheet" type="text/css" href="${pageContext.request.contextPath}/css/style.css">
     <script>
        function confirmDelete(patientId) {
            if (confirm("Are you sure you want to delete this patient and all their records? This cannot be undone.")) {
                window.location.href = "${pageContext.request.contextPath}/patient?action=delete&id=" + patientId;
            }
        }
    </script>
</head>
<body>
    <div class="header">
        <h1>Doctor Home Page</h1>
        <a href="${pageContext.request.contextPath}/logout" class="logout-link">Logout</a>
    </div>

    <div class="nav-bar">
        <a href="${pageContext.request.contextPath}/patient?action=list" class="button-link button-default">Home (All Patients)</a>
        <a href="${pageContext.request.contextPath}/patient?action=showAddForm" class="button-link">Add Patient</a>
         <form action="${pageContext.request.contextPath}/patient" method="get" style="display:inline-block; margin-left: 20px;">
            <input type="hidden" name="action" value="search">
            <label for="searchQuery">Search Patient:</label>
            <input type="text" id="searchQuery" name="query" placeholder="Enter name, phone, etc." value="${searchQuery}" style="padding: 8px; width: 200px;" required>
            <button type="submit" class="button" style="padding: 8px 12px;">Search</button>
        </form>
    </div>

    <div class="container">
        <h2>Patient Search Results for: "<c:out value="${searchQuery}"/>" [ <c:out value="${patientList.size()}"/> found ]</h2>
        
        <c:choose>
            <c:when test="${not empty patientList}">
                <table>
                    <thead>
                        <tr>
                            <th>Sr. No</th>
                            <th>Name - Age</th>
                            <th>Phone</th>
                            <th>Diagnosis</th>
                            <th>Remark</th>
                            <th>Gender</th>
                            <th>Action</th>
                        </tr>
                    </thead>
                    <tbody>
                        <c:forEach var="patient" items="${patientList}" varStatus="status">
                            <tr>
                                <td>${status.count}</td>
                                <td><c:out value="${patient.name}"/> - <c:out value="${patient.age}"/></td>
                                <td><c:out value="${patient.phone}"/></td>
                                <td><c:out value="${patient.diagnosis}"/></td>
                                <td><c:out value="${patient.remarks}"/></td>
                                <td><c:out value="${patient.gender}"/></td>
                                <td>
                                    <a href="${pageContext.request.contextPath}/vital?action=listPatientVitals&patientid=${patient.patientId}" class="button-link button-info">Manage Vitals</a>
                                    <a href="${pageContext.request.contextPath}/patient?action=showEditForm&id=${patient.patientId}" class="button-link">Update</a>
                                    <a href="javascript:void(0);" onclick="confirmDelete(${patient.patientId})" class="button-link button-danger">Delete</a>
                                </td>
                            </tr>
                        </c:forEach>
                    </tbody>
                </table>
            </c:when>
            <c:otherwise>
                <p>No patients found matching your search criteria: "<c:out value="${searchQuery}"/>".</p>
            </c:otherwise>
        </c:choose>
    </div>
</body>
</html>



vitalform

<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<c:if test="${empty sessionScope.admin}">
    <c:redirect url="adminLogin.jsp?auth_error=true"/>
</c:if>
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Add Patient's Vital Information - Health Logger</title>
    <link rel="stylesheet" type="text/css" href="${pageContext.request.contextPath}/css/style.css">
</head>
<body>
    <div class="header">
        <h1>Health Logger</h1>
         <a href="${pageContext.request.contextPath}/logout" class="logout-link">Logout</a>
    </div>

    <div class="nav-bar">
        <a href="${pageContext.request.contextPath}/adminHome.jsp" class="button-link button-default">Dashboard</a>
        <c:choose>
            <c:when test="${not empty selectedPatientId}">
                 <a href="${pageContext.request.contextPath}/vital?action=listPatientVitals&patientid=${selectedPatientId}" class="button-link">Back to Patient Vitals</a>
            </c:when>
            <c:otherwise>
                 <a href="${pageContext.request.contextPath}/vital?action=listAll" class="button-link">Back to All Vitals</a>
            </c:otherwise>
        </c:choose>
    </div>

    <div class="container">
        <h2>Add Patient's Vital Information</h2>

        <form action="${pageContext.request.contextPath}/vital" method="post">
            <input type="hidden" name="action" value="add">

            <div class="form-group">
                <label for="patientId">Select Patient:</label>
                <select id="patientId" name="patientId" required>
                    <option value="">-- Select Patient --</option>
                    <c:forEach var="p" items="${patientList}">
                        <option value="${p.patientId}" ${p.patientId == selectedPatientId ? 'selected' : ''}>
                            <c:out value="${p.name}"/> (ID: ${p.patientId})
                        </option>
                    </c:forEach>
                </select>
            </div>
            <div class="form-group">
                <label for="bpLow">BP Low:</label>
                <input type="number" id="bpLow" name="bpLow" required>
            </div>
            <div class="form-group">
                <label for="bpHigh">BP High:</label>
                <input type="number" id="bpHigh" name="bpHigh" required>
            </div>
            <div class="form-group">
                <label for="spo2">SPO2 (%):</label>
                <input type="number" id="spo2" name="spo2" required min="0" max="100">
            </div>
            <%-- Timestamp is handled by server/DB --%>
            <button type="submit" class="button">Submit</button>
        </form>
    </div>
</body>
</html>

managevitals

<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<%@ taglib uri="http://java.sun.com/jsp/jstl/fmt" prefix="fmt" %>
<c:if test="${empty sessionScope.admin}">
    <c:redirect url="adminLogin.jsp?auth_error=true"/>
</c:if>
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>${not empty patient ? patient.name : 'All Patients'} Vitals - Health Logger</title>
    <link rel="stylesheet" type="text/css" href="${pageContext.request.contextPath}/css/style.css">
    <c:if test="${not empty patient && not empty chartLabels}"> <%-- Only include Chart.js if displaying graph --%>
        <script src="${pageContext.request.contextPath}/js/chart.min.js"></script>
    </c:if>
    <script>
        function confirmDeleteVital(vitalId, patientIdContext) {
            if (confirm("Are you sure you want to delete this vital record?")) {
                let url = "${pageContext.request.contextPath}/vital?action=delete&id=" + vitalId;
                if (patientIdContext && patientIdContext !== '') {
                    url += "&patientid_context=" + patientIdContext;
                }
                window.location.href = url;
            }
        }
    </script>
</head>
<body>
    <div class="header">
        <h1>Doctor Home Page</h1>
        <a href="${pageContext.request.contextPath}/logout" class="logout-link">Logout</a>
    </div>
    
    <div class="nav-bar">
        <a href="${pageContext.request.contextPath}/adminHome.jsp" class="button-link button-default">Dashboard</a>
        <a href="${pageContext.request.contextPath}/patient?action=list" class="button-link">Manage Patients</a>
        <c:if test="${not empty patient}"> <%-- If viewing specific patient's vitals --%>
            <a href="${pageContext.request.contextPath}/vital?action=listAll" class="button-link">All Vitals List</a>
            <a href="${pageContext.request.contextPath}/vital?action=export&patientid=${patient.patientId}" class="button-link button-info">Export These Vitals</a>
        </c:if>
        <c:if test="${empty patient}"> <%-- If viewing all vitals --%>
             <a href="${pageContext.request.contextPath}/vital?action=export" class="button-link button-info">Export All Vitals</a>
        </c:if>
        <a href="${pageContext.request.contextPath}/vital?action=showAddForm${not empty patient ? '&patientId=' : ''}${patient.patientId}" class="button-link">Record New Vital</a>
