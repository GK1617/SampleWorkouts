import React from 'react'
import { useEffect } from 'react'
import { useState } from 'react'
import { deleteEmployee, getEmployee, listEmployee } from './Employeeservice'
import { useNavigate } from 'react-router-dom'

const ListEmployeeComponent = () => {

    const[employees, setEmployees]=useState([])    

    const navigator=useNavigate();

    useEffect(()=>{
        getAllEmployees();
       
    },[])

    function getAllEmployees(){
        listEmployee().then((response)=>{
            getEmployee(response.data)
            console.log(response.data)
        }).catch(error=>{
            console.error(error);
        })
    }

    function addNewEmployee(){
        navigator('/add-employee')
    }

    function updateEmployee(){
        navigator(`/edit-employee/${id}`)
    }
    function removeEmployee(id){
        console.log(id);
        deleteEmployee(id).then((response)=>{
             getAllEmployees();
        }).catch(error=>{
            console.error(error);
        })
    }

  return (
    <div className='container'>
        <button class="btn btn-primary mb-2" onClick={addNewEmployee}>Add Employee</button>
        <h2 className='text-center'>List of Employees</h2>
        <table className="table table-success table-bordered">
            <thead>
                <tr>
                    <th>Employee Id</th>
                    <th>Employee Name</th>
                    <th>Employee Email</th>
                    <th>Employee PhoneNumber</th>
                    <th>Actions</th>
                </tr>
            </thead>
            <tbody>{
                employees.map(employee=>
                    <tr key={employee.id}>
                    <td>{employee.id}</td>
                    <td>{employee.name}</td>
                    <td>{employee.email}</td>
                    <td>{employee.phoneNumber}</td>
                    <td>
                        <button className='btn btn-info' onClick={()=>updateEmployee(employee.id)}>Update</button>
                        <button className='btn btn-danger' onClick={()=>removeEmployee(employee.id)}
                            style={{
                                marginLeft:'10px'
                            }}>Delete</button>

                    </td>
                    </tr>
                )}
                
            </tbody>
        </table>
    </div>
  )
}

export default ListEmployeeComponent
