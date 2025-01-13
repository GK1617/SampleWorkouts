import React, { useEffect, useState } from "react";
import { createEmployee, getEmployee, updateEmployee } from "./Employeeservice";
import { useNavigate, useParams } from "react-router-dom";

const EmployeeComponent=()=>{

    const[name, setName]= useState('')
    const[email, setemail]= useState('')
    const[phoneNumber, setPhoneNumber]= useState('')
    const {id}=useParams();
    const[errors, setErrors]=useState({
        name:'',
        email:'',
        phoneNumber:''
    })

     useEffect(()=>{
        if(id){
                getEmployee(id).then((response)=>{
                setName(response.data.name)
                setemail(response.data.email)
                setPhoneNumber(response.data.phoneNumber)
                }).catch(error=>{
                console.error(error);
            })
        }
    },[id])
    

    const navigator=useNavigate();

    function saveOrUpdateEmployee(e){
        e.preventDefault();

        if(validationForm()){
            const employee={name,email,phoneNumber}
            console.log(employee);

            if(id){
                updateEmployee(id,employee).then((response)=>{
                    console.log(response.data);
                    navigator('/employees');
                }).catch(error=>{
                    console.error(error)
                })
            }
            else{
                createEmployee(employee).then((response)=>{
                    console.log(response.data)
                    navigator('/employees')
                }).catch(error=>{
                    console.error(error)
                })
            }
            
        }
    }
       
        
    function validationForm(){
        let valid=true;
            //... we use spread operator to copy object into another object
        const errorsCopy={...errors}

        if(name.trim()){
            errorsCopy.name='';
        }else{
            errorsCopy.name='Name is required';
            valid=false;
        }
        
        if(email.trim()){
            errorsCopy.email='';
        }else{
            errorsCopy.email='Email is required';
            valid=false;
        }
        
        if(phoneNumber.trim()){
            errorsCopy.phoneNumber='';
        }else{
            errorsCopy.phoneNumber='phoneNumber is required';
            valid=false;
        }
        setErrors(errorsCopy)
        return valid;
    }
    function pageTitle(){
        if(id){
            return  <h2 className='text-center'>Update Employee</h2>
        }
        else{
            return <h2 className='text-center'>Add Employee</h2>
        }
    }
    return (
        <div className='container'>
            <br/> <br/>
            <div className='row'>
                <div className='card col-md-6 offset-md-3 offset-md-3'>
                   {
                    pageTitle()
                   }
                    <br/> <br/>
                    <div className='card-body'>
                        <form>
                            <div className='form-group mb-2'>
                                <label className='form-label'> Name: </label>
                                <input
                                type='text'
                                placeholder="Enter your Name"
                                name='(name)'
                                value={name}
                                className={`form-control ${ errors.name ?'is-invalid' :''}`}
                                onChange={(e)=>setName(e.target.value)}>
                                </input>
                                {errors.name && <div className="invalid-feedback">{errors.name}</div>}
                            </div>
                            <div className='form-group mb-2'>
                                <label className='form-label'> Email: </label>
                                <input
                                type='email'
                                placeholder="Enter your Name"
                                name='(email)'
                                value={email}
                                className={`form-control ${ errors.email ?'is-invalid' :''}`}
                                onChange={(e)=>setemail(e.target.value)}>
                                </input>
                                {errors.email && <div className="invalid-feedback">{errors.email}</div>}
                            </div>
                            <div className='form-group mb-2'>
                                <label className='form-label'> PhoneNumber: </label>
                                <input
                                type='number'
                                placeholder="Enter your PhoneNumber"
                                name='(phoneNuber)'
                                value={phoneNumber}
                                className={`form-control ${ errors.phoneNumber ?'is-invalid' :''}`}
                                onChange={(e)=>setPhoneNumber(e.target.value)}>
                                </input>
                                {errors.phoneNumber && <div className="invalid-feedback">{errors.phoneNumber}</div>}
                            </div>
                            <button className='btn btn-success' onClick={saveOrUpdateEmployee}>Submit</button>
                        </form>

                    </div>

                </div>
            </div>
        </div>
    )
}
export default EmployeeComponent
