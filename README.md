# mlaregular



<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<c:if test="${empty sessionScope.currentAdmin}">
    <c:redirect url="adminLogin.jsp?error=Please login first"/>
</c:if>
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Admin Dashboard - Health Logger</title>
     <style>
        body { font-family: Arial, sans-serif; margin: 0; padding: 0; background-color: #f9f9f9; }
        .header { background-color: #333; color: white; padding: 15px 20px; text-align: center; }
        .header h1 { margin: 0; }
        .header .user-info { float: right; font-size: 0.9em; }
        .header .user-info a { color: #ffc107; text-decoration: none; }
        .nav-container { text-align: center; margin-top: 50px; }
        .nav-button {
            display: inline-block;
            background-color: #007bff;
            color: white;
            padding: 20px 40px;
            margin: 10px;
            text-decoration: none;
            border-radius: 8px;
            font-size: 1.2em;
            transition: background-color 0.3s;
        }
        .nav-button:hover { background-color: #0056b3; }
        .app-info { text-align:center; margin-top: 20px; color: #555;}
    </style>
</head>
<body>
    <div class="header">
        <div class="user-info">
            Logged in as: ${sessionScope.currentAdmin.email} | <a href="adminServlet?action=logout">Logout</a>
        </div>
        <h1>Health Logger</h1>
    </div>
    
    <div class="app-info">
        <p>Developed by John Peterson</p>
    </div>

    <div class="nav-container">
        <a href="patientServlet?action=list" class="nav-button">Manage Patients</a>
        <a href="vitalServlet?action=list" class="nav-button">Manage Vitals</a>
    </div>
</body>
</html>
