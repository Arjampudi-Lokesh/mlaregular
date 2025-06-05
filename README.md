<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<c:if test="${empty sessionScope.currentAdmin}">
    <c:redirect url="adminLogin.jsp?error=Please login first"/>
</c:if>
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>${param.mode == 'edit' ? 'Update' : 'Add'} Patient - Health Logger</title>
    <style>
        body { font-family: Arial, sans-serif; background-color: #f4f4f4; margin: 20px; }
        .container { background-color: #fff; padding: 20px; border-radius: 8px; box-shadow: 0 0 10px rgba(0,0,0,0.1); max-width: 600px; margin: auto; }
        h2 { color: #333; text-align: center; }
        .form-group { margin-bottom: 15px; }
        .form-group label { display: block; margin-bottom: 5px; color: #555; }
        .form-group input[type="text"],
        .form-group input[type="number"],
        .form-group input[type="email"],
        .form-group textarea,
        .form-group select { width: calc(100% - 16px); padding: 8px; border: 1px solid #ddd; border-radius: 4px; }
        .form-group textarea { resize: vertical; min-height: 80px; }
        .form-group .radio-group label { margin-right: 15px; font-weight: normal; }
        .form-group .radio-group input[type="radio"] { margin-right: 5px; }
        .btn-submit { background-color: #5cb85c; color: white; padding: 10px 15px; border: none; border-radius: 4px; cursor: pointer; font-size: 16px; }
        .btn-submit:hover { background-color: #4cae4c; }
        .btn-back { text-decoration: none; color: #007bff; margin-right:10px; }
    </style>
</head>
<body>
    <div class="container">
        <a href="patientServlet?action=list" class="btn-back">« Back to List</a>
        <h2>${param.mode == 'edit' ? 'Update' : 'Add'} Patient Information</h2>
        
        <form action="patientServlet" method="post">
            <c:if test="${param.mode == 'edit'}">
                <input type="hidden" name="action" value="update"/>
                <input type="hidden" name="id" value="${patient.id}"/>
            </c:if>
            <c:if test="${param.mode == 'add'}">
                <input type="hidden" name="action" value="add"/>
            </c:if>

            <div class="form-group">
                <label for="name">Name:</label>
                <input type="text" id="name" name="name" value="${patient.name}" required>
            </div>
            <div class="form-group">
                <label for="email">Email:</label> <!-- Not in PDF model, but screenshot has it -->
                <input type="email" id="email" name="email" value="${patient.email}"> <!-- Assuming email might be a field -->
            </div>
            <div class="form-group">
                <label for="phone">Phone:</label>
                <input type="text" id="phone" name="phone" value="${patient.phone}">
            </div>
            <div class="form-group">
                <label for="age">Age:</label>
                <input type="number" id="age" name="age" value="${patient.age}">
            </div>
            <div class="form-group">
                <label>Gender:</label>
                <div class="radio-group">
                    <label><input type="radio" name="gender" value="Male" ${patient.gender == 'Male' ? 'checked' : ''}> Male</label>
                    <label><input type="radio" name="gender" value="Female" ${patient.gender == 'Female' ? 'checked' : ''}> Female</label>
                    <label><input type="radio" name="gender" value="Other" ${patient.gender == 'Other' ? 'checked' : ''}> Other</label>
                </div>
            </div>
            <div class="form-group">
                <label for="diagnosis">Diagnosis:</label>
                <textarea id="diagnosis" name="diagnosis">${patient.diagnosis}</textarea>
            </div>
            <div class="form-group">
                <label for="remarks">Remarks:</label>
                <textarea id="remarks" name="remarks">${patient.remarks}</textarea>
            </div>
            <button type="submit" class="btn-submit">${param.mode == 'edit' ? 'Update' : 'Add'} Patient</button>
        </form>
    </div>
</body>
</html>
