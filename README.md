<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<c:if test="${empty sessionScope.currentAdmin}">
    <c:redirect url="adminLogin.jsp?error=Please login first"/>
</c:if>
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Manage Patients - Health Logger</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 0; padding: 0; background-color: #f9f9f9; }
        .header { background-color: #333; color: white; padding: 10px 20px; display: flex; justify-content: space-between; align-items: center; }
        .header h1 { margin: 0; font-size: 1.5em; }
        .header a { color: #ffc107; text-decoration: none; margin-left: 15px; }
        .container { padding: 20px; }
        .actions { margin-bottom: 20px; }
        .actions a, .actions button { 
            background-color: #007bff; color: white; padding: 8px 12px; text-decoration: none; 
            border-radius: 4px; margin-right: 10px; border:none; cursor:pointer; font-size: 0.9em;
        }
        .actions a.add { background-color: #28a745; }
        .actions a:hover, .actions button:hover { opacity: 0.9; }
        .search-form { margin-bottom: 20px; }
        .search-form input[type="text"] { padding: 8px; border-radius: 4px; border: 1px solid #ddd; width: 250px; }
        .search-form button { padding: 8px 12px; }
        table { width: 100%; border-collapse: collapse; background-color: white; box-shadow: 0 0 5px rgba(0,0,0,0.1); }
        th, td { border: 1px solid #ddd; padding: 10px; text-align: left; }
        th { background-color: #f2f2f2; }
        td a { margin-right: 5px; text-decoration: none; padding: 3px 7px; border-radius:3px; }
        td a.vitals { background-color: #17a2b8; color:white; }
        td a.update { background-color: #ffc107; color:black; }
        td a.delete { background-color: #dc3545; color:white; }
    </style>
</head>
<body>
    <div class="header">
        <h1>Doctor Home Page</h1>
        <div>
            <a href="adminHome.jsp">Dashboard Home</a>
            <a href="adminServlet?action=logout">Logout</a>
        </div>
    </div>

    <div class="container">
        <h2>Patients [ <c:out value="${listPatient.size()}"/> ]</h2>
        
        <div class="actions">
            <a href="patientServlet?action=new" class="add">Add Patient</a>
            <a href="vitalServlet?action=list">Manage Vitals</a>
        </div>

        <form action="patientServlet" method="get" class="search-form">
            <input type="hidden" name="action" value="search"/>
            <input type="text" name="searchTerm" placeholder="Search Patient by Name, Diagnosis..." value="${searchTerm}"/>
            <button type="submit">Search</button>
             <c:if test="${not empty searchTerm}">
                <a href="patientServlet?action=list">Clear Search</a>
            </c:if>
        </form>

        <table>
            <thead>
                <tr>
                    <th>Sr. No</th>
                    <th>Name - Age</th>
                    <th>Email</th> <!-- As per screenshot, though not in PDF patient model -->
                    <th>Phone</th>
                    <th>Diagnosis</th>
                    <th>Remark</th>
                    <th>Gender</th>
                    <th>Action</th>
                </tr>
            </thead>
            <tbody>
                <c:forEach var="patient" items="${listPatient}" varStatus="status">
                    <tr>
                        <td>${status.count}</td>
                        <td>${patient.name} - ${patient.age}</td>
                        <td>${patient.email}</td> <!-- Assuming patient bean has email -->
                        <td>${patient.phone}</td>
                        <td>${patient.diagnosis}</td>
                        <td>${patient.remarks}</td>
                        <td>${patient.gender}</td>
                        <td>
                            <a href="vitalServlet?action=listForPatient&patientId=${patient.id}" class="vitals">Manage Vitals</a>
                            <a href="patientServlet?action=edit&id=${patient.id}" class="update">Update</a>
                            <a href="patientServlet?action=delete&id=${patient.id}" class="delete" onclick="return confirm('Are you sure you want to delete this patient?');">Delete</a>
                        </td>
                    </tr>
                </c:forEach>
                <c:if test="${empty listPatient}">
                    <tr><td colspan="8">No patients found.</td></tr>
                </c:if>
            </tbody>
        </table>
    </div>
</body>
</html>
