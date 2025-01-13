import axios from "axios";

const Rest_API_Base_Url='http://localhost:8080/api/user/1' ;// Here we need to insert the Controller api's from SpringBoot

export const listEmployee=()=> {
    return axios.get(Rest_API_Base_Url);
}
export const createEmployee=(employee)=> axios.post('http://localhost:8080/api/addEmp',employee);
export const getEmployee=(employeeId)=>axios.get('http://localhost:8080/api/getusers'+'/'+employeeId);
export const updateEmployee=(employeeId,employee)=>axios.put(Rest_API_Base_Url +'/'+employeeId,employee);
export const deleteEmployee=(employeeId)=>axios.delete(Rest_API_Base_Url +'/'+employeeId);
