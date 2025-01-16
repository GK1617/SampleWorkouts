import { useContext, useEffect, useState } from "react";
import { Link, useNavigate } from "react-router-dom";
import { toast } from "react-toastify";
import axios from "axios";
import { listEmployee } from "./Employeeservice";

//toast.configure();

const Login = ({setAuth}) => {
    const [username, usernameupdate] = useState('');
    const [password, passwordupdate] = useState('');
    const usenavigate=useNavigate();
    const ProceedLogin= async(e)=>{
        e.preventDefault();
        try{
            const response=await axios.fetch(`http://localhost:3000/users`); //?username=${username}&password=${password}
            const users=await response.json();
            // if(response.data.length>0){
            //     toast.success('Login Successful');
            //     console.log('done login')
            //     setAuth(true);
            //     usenavigate('/add-employee')
            // }else{
            //     toast.error('Invalid Credentials')
            //     console.log('login failed')
            // }

            if(users){
                setAuth(true);
                usenavigate('/add-employee')
            }else{
                toast.error('Invalid Username and password');
                console.log('invalid');
            }
            usernameupdate('');
            passwordupdate('');
        }catch(error){
            console.error('Error logging in:' ,error);
            toast.error('Failed tologin');
        }
    };

   
    return (
        <div className="row">
            <div className="offset-lg-3 col-lg-6" style={{ marginTop: '100px' }}>
                <form onSubmit={ProceedLogin} className="container">
                    <div className="card">
                        <div className="card-header">
                            <h2>User Login</h2>
                        </div>
                        <div className="card-body">
                            <div className="form-group">
                                <label>User Name <span className="errmsg">*</span></label>
                                <input value={username} onChange={e => usernameupdate(e.target.value)} className="form-control"></input>
                            </div>
                            <div className="form-group">
                                <label>Password <span className="errmsg">*</span></label>
                                <input type="password" value={password} onChange={e => passwordupdate(e.target.value)} className="form-control"></input>
                            </div>
                        </div>
                        <div className="card-footer">
                            <button type="submit" className="btn btn-primary">Login</button> |
                            <Link className="btn btn-success"  to={'/register'}>New User</Link> 
                        </div>
                    </div>
                </form>
            </div>
        </div>
    );
}

export default Login;
