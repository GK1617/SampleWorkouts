import axios from 'axios';

// Base configuration
const BASE_URL = 'http://localhost:8080/api';

// API endpoints
const ENDPOINTS = {
    LIST_EMPLOYEES: '/user/1',
    ADD_EMPLOYEE: '/addEmp',
    GET_EMPLOYEE: '/getusers',
    UPDATE_EMPLOYEE: '/user/1',
    DELETE_EMPLOYEE: '/user/1'
};

// Create axios instance with default config
const apiClient = axios.create({
    baseURL: BASE_URL,
    headers: {
        'Content-Type': 'application/json'
    }
});

// Error handling wrapper
const handleApiError = async (apiCall) => {
    try {
        const response = await apiCall();
        return response;
    } catch (error) {
        console.error('API Error:', error.response?.data || error.message);
        throw error;
    }
};

// Employee service methods
export const listEmployee = () => {
    return handleApiError(() => apiClient.get(ENDPOINTS.LIST_EMPLOYEES));
};

export const createEmployee = (employee) => {
    return handleApiError(() => apiClient.post(ENDPOINTS.ADD_EMPLOYEE, employee));
};

export const getEmployee = (employeeId) => {
    return handleApiError(() => apiClient.get(`${ENDPOINTS.GET_EMPLOYEE}/${employeeId}`));
};

export const updateEmployee = (employeeId, employee) => {
    return handleApiError(() => apiClient.put(`${ENDPOINTS.UPDATE_EMPLOYEE}/${employeeId}`, employee));
};

export const deleteEmployee = (employeeId) => {
    return handleApiError(() => apiClient.delete(`${ENDPOINTS.DELETE_EMPLOYEE}/${employeeId}`));
};