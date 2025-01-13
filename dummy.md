import React, { useEffect, useState } from "react";
import { createEmployee, getEmployee, updateEmployee } from "./Employeeservice";
import { useNavigate, useParams } from "react-router-dom";

const EmployeeComponent = () => {
    const [name, setName] = useState('');
    const [email, setEmail] = useState(''); // Fixed naming convention
    const [phoneNumber, setPhoneNumber] = useState('');
    const { id } = useParams();
    const navigate = useNavigate(); // Fixed naming convention

    const [errors, setErrors] = useState({
        name: '',
        email: '',
        phoneNumber: ''
    });

    useEffect(() => {
        if (id) {
            getEmployee(id)
                .then((response) => {
                    const { name, email, phoneNumber } = response.data;
                    setName(name);
                    setEmail(email);
                    setPhoneNumber(phoneNumber);
                })
                .catch(error => {
                    console.error('Error fetching employee:', error);
                });
        }
    }, [id]);

    const validateEmail = (email) => {
        const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
        return emailRegex.test(email);
    };

    const validatePhoneNumber = (phone) => {
        const phoneRegex = /^\d{10}$/; // Assumes 10-digit phone number
        return phoneRegex.test(phone);
    };

    const validateForm = () => {
        let valid = true;
        const errorsCopy = { ...errors };

        // Name validation
        if (!name.trim()) {
            errorsCopy.name = 'Name is required';
            valid = false;
        } else if (name.trim().length < 2) {
            errorsCopy.name = 'Name must be at least 2 characters';
            valid = false;
        } else {
            errorsCopy.name = '';
        }

        // Email validation
        if (!email.trim()) {
            errorsCopy.email = 'Email is required';
            valid = false;
        } else if (!validateEmail(email)) {
            errorsCopy.email = 'Please enter a valid email address';
            valid = false;
        } else {
            errorsCopy.email = '';
        }

        // Phone number validation
        if (!phoneNumber.trim()) {
            errorsCopy.phoneNumber = 'Phone number is required';
            valid = false;
        } else if (!validatePhoneNumber(phoneNumber)) {
            errorsCopy.phoneNumber = 'Please enter a valid 10-digit phone number';
            valid = false;
        } else {
            errorsCopy.phoneNumber = '';
        }

        setErrors(errorsCopy);
        return valid;
    };

    const saveOrUpdateEmployee = async (e) => {
        e.preventDefault();

        if (validateForm()) {
            const employee = { name, email, phoneNumber };

            try {
                if (id) {
                    await updateEmployee(id, employee);
                } else {
                    await createEmployee(employee);
                }
                navigate('/employees');
            } catch (error) {
                console.error('Error saving employee:', error);
                // You might want to show an error message to the user here
            }
        }
    };

    const getPageTitle = () => {
        return (
            <h2 className='text-center'>
                {id ? 'Update Employee' : 'Add Employee'}
            </h2>
        );
    };

    return (
        <div className='container'>
            <div className='row mt-4'>
                <div className='card col-md-6 offset-md-3'>
                    {getPageTitle()}
                    <div className='card-body'>
                        <form onSubmit={saveOrUpdateEmployee}>
                            <div className='form-group mb-3'>
                                <label className='form-label'>Name:</label>
                                <input
                                    type='text'
                                    placeholder="Enter employee name"
                                    name='name'
                                    value={name}
                                    className={`form-control ${errors.name ? 'is-invalid' : ''}`}
                                    onChange={(e) => setName(e.target.value)}
                                />
                                {errors.name && <div className="invalid-feedback">{errors.name}</div>}
                            </div>

                            <div className='form-group mb-3'>
                                <label className='form-label'>Email:</label>
                                <input
                                    type='email'
                                    placeholder="Enter email address"
                                    name='email'
                                    value={email}
                                    className={`form-control ${errors.email ? 'is-invalid' : ''}`}
                                    onChange={(e) => setEmail(e.target.value)}
                                />
                                {errors.email && <div className="invalid-feedback">{errors.email}</div>}
                            </div>

                            <div className='form-group mb-3'>
                                <label className='form-label'>Phone Number:</label>
                                <input
                                    type='tel'
                                    placeholder="Enter 10-digit phone number"
                                    name='phoneNumber'
                                    value={phoneNumber}
                                    className={`form-control ${errors.phoneNumber ? 'is-invalid' : ''}`}
                                    onChange={(e) => setPhoneNumber(e.target.value)}
                                />
                                {errors.phoneNumber && <div className="invalid-feedback">{errors.phoneNumber}</div>}
                            </div>

                            <button type="submit" className='btn btn-primary'>
                                {id ? 'Update' : 'Save'} Employee
                            </button>
                        </form>
                    </div>
                </div>
            </div>
        </div>
    );
};

export default EmployeeComponent;