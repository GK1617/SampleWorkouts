// src/context/AuthContext.js
import { createContext, useContext, useState } from 'react';

const AuthContext = createContext(null);

export const AuthProvider = ({ children }) => {
  const [user, setUser] = useState(null);

  const login = (userData) => {
    setUser(userData);
    localStorage.setItem('user', JSON.stringify(userData));
  };

  const logout = () => {
    setUser(null);
    localStorage.removeItem('user');
  };

  return (
    <AuthContext.Provider value={{ user, login, logout }}>
      {children}
    </AuthContext.Provider>
  );
};

export const useAuth = () => useContext(AuthContext);

// src/components/ProtectedRoute.js
import { Navigate } from 'react-router-dom';
import { useAuth } from '../context/AuthContext';

const ProtectedRoute = ({ children }) => {
  const { user } = useAuth();
  return user ? children : <Navigate to="/login" />;
};

// src/components/Dashboard.js
import React from 'react';
import { AppBar, Toolbar, Typography, Button, Container, Box } from '@mui/material';
import { useAuth } from '../context/AuthContext';
import { useNavigate } from 'react-router-dom';

const Dashboard = () => {
  const { user, logout } = useAuth();
  const navigate = useNavigate();

  return (
    <>
      <AppBar position="static">
        <Toolbar>
          <Typography variant="h6" component="div" sx={{ flexGrow: 1 }}>
            Welcome, {user?.name}
          </Typography>
          <Button color="inherit" onClick={() => navigate('/employees/add')}>
            Add Employee
          </Button>
          <Button color="inherit" onClick={() => navigate('/employees')}>
            Employee List
          </Button>
          <Button color="inherit" onClick={logout}>
            Logout
          </Button>
        </Toolbar>
      </AppBar>
      <Container>
        <Box sx={{ mt: 4 }}>
          <Typography variant="h4" gutterBottom>
            Dashboard
          </Typography>
        </Box>
      </Container>
    </>
  );
};

// src/components/AddEmployee.js
import React, { useState } from 'react';
import { Card, CardContent, TextField, Button, Box, Typography } from '@mui/material';
import axios from 'axios';

const AddEmployee = () => {
  const [employee, setEmployee] = useState({
    name: '',
    email: '',
    position: '',
    department: ''
  });

  const handleSubmit = async (e) => {
    e.preventDefault();
    try {
      await axios.post('http://localhost:8080/api/employees', employee);
      // Handle success (redirect or show message)
    } catch (error) {
      console.error('Error adding employee:', error);
    }
  };

  return (
    <Box sx={{ maxWidth: 600, mx: 'auto', mt: 4 }}>
      <Card>
        <CardContent>
          <Typography variant="h5" gutterBottom>
            Add New Employee
          </Typography>
          <form onSubmit={handleSubmit}>
            <TextField
              fullWidth
              label="Name"
              margin="normal"
              value={employee.name}
              onChange={(e) => setEmployee({ ...employee, name: e.target.value })}
            />
            <TextField
              fullWidth
              label="Email"
              margin="normal"
              value={employee.email}
              onChange={(e) => setEmployee({ ...employee, email: e.target.value })}
            />
            <TextField
              fullWidth
              label="Position"
              margin="normal"
              value={employee.position}
              onChange={(e) => setEmployee({ ...employee, position: e.target.value })}
            />
            <TextField
              fullWidth
              label="Department"
              margin="normal"
              value={employee.department}
              onChange={(e) => setEmployee({ ...employee, department: e.target.value })}
            />
            <Button
              type="submit"
              variant="contained"
              color="primary"
              sx={{ mt: 2 }}
            >
              Add Employee
            </Button>
          </form>
        </CardContent>
      </Card>
    </Box>
  );
};

// src/components/EmployeeList.js
import React, { useState, useEffect } from 'react';
import { Grid, Card, CardContent, Typography, Button, Dialog, DialogContent } from '@mui/material';
import axios from 'axios';

const EmployeeList = () => {
  const [employees, setEmployees] = useState([]);
  const [selectedEmployee, setSelectedEmployee] = useState(null);
  const [openDialog, setOpenDialog] = useState(false);

  useEffect(() => {
    fetchEmployees();
  }, []);

  const fetchEmployees = async () => {
    try {
      const response = await axios.get('http://localhost:8080/api/employees');
      setEmployees(response.data);
    } catch (error) {
      console.error('Error fetching employees:', error);
    }
  };

  const handleDelete = async (id) => {
    try {
      await axios.delete(`http://localhost:8080/api/employees/${id}`);
      fetchEmployees();
    } catch (error) {
      console.error('Error deleting employee:', error);
    }
  };

  return (
    <>
      <Grid container spacing={3} sx={{ p: 3 }}>
        {employees.map((employee) => (
          <Grid item xs={12} sm={6} md={4} key={employee.id}>
            <Card sx={{ cursor: 'pointer' }} onClick={() => {
              setSelectedEmployee(employee);
              setOpenDialog(true);
            }}>
              <CardContent>
                <Typography variant="h6">{employee.name}</Typography>
                <Typography color="textSecondary">{employee.position}</Typography>
                <Typography variant="body2">{employee.department}</Typography>
              </CardContent>
            </Card>
          </Grid>
        ))}
      </Grid>

      <Dialog open={openDialog} onClose={() => setOpenDialog(false)} maxWidth="sm" fullWidth>
        <DialogContent>
          {selectedEmployee && (
            <Card>
              <CardContent>
                <Typography variant="h5">{selectedEmployee.name}</Typography>
                <Typography color="textSecondary">{selectedEmployee.email}</Typography>
                <Typography>{selectedEmployee.position}</Typography>
                <Typography>{selectedEmployee.department}</Typography>
                <Box sx={{ mt: 2 }}>
                  <Button
                    variant="contained"
                    color="primary"
                    sx={{ mr: 1 }}
                    onClick={() => {/* Handle edit */}}
                  >
                    Edit
                  </Button>
                  <Button
                    variant="contained"
                    color="error"
                    onClick={() => handleDelete(selectedEmployee.id)}
                  >
                    Delete
                  </Button>
                </Box>
              </CardContent>
            </Card>
          )}
        </DialogContent>
      </Dialog>
    </>
  );
};

// src/components/EditEmployee.js
import React, { useState, useEffect } from 'react';
import { Card, CardContent, TextField, Button, Box, Typography } from '@mui/material';
import axios from 'axios';

const EditEmployee = ({ employee, onSave, onCancel }) => {
  const [formData, setFormData] = useState(employee);

  const handleSubmit = async (e) => {
    e.preventDefault();
    try {
      await axios.put(`http://localhost:8080/api/employees/${employee.id}`, formData);
      onSave();
    } catch (error) {
      console.error('Error updating employee:', error);
    }
  };

  return (
    <Card>
      <CardContent>
        <Typography variant="h5" gutterBottom>
          Edit Employee
        </Typography>
        <form onSubmit={handleSubmit}>
          <TextField
            fullWidth
            label="Name"
            margin="normal"
            value={formData.name}
            onChange={(e) => setFormData({ ...formData, name: e.target.value })}
          />
          <TextField
            fullWidth
            label="Email"
            margin="normal"
            value={formData.email}
            onChange={(e) => setFormData({ ...formData, email: e.target.value })}
          />
          <TextField
            fullWidth
            label="Position"
            margin="normal"
            value={formData.position}
            onChange={(e) => setFormData({ ...formData, position: e.target.value })}
          />
          <TextField
            fullWidth
            label="Department"
            margin="normal"
            value={formData.department}
            onChange={(e) => setFormData({ ...formData, department: e.target.value })}
          />
          <Box sx={{ mt: 2 }}>
            <Button type="submit" variant="contained" color="primary" sx={{ mr: 1 }}>
              Save Changes
            </Button>
            <Button variant="outlined" onClick={onCancel}>
              Cancel
            </Button>
          </Box>
        </form>
      </CardContent>
    </Card>
  );
};
