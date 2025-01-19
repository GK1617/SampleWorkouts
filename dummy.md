import React, { useState } from 'react'
import { Box,Typography,TextField, Button } from '@mui/material'
import LoginIcon from '@mui/icons-material/Login';
import HowToRegIcon from '@mui/icons-material/HowToReg';
import { useNavigate } from 'react-router-dom';

const Login = () => {
    const navigate=useNavigate();
    const [isSingup, setIsSingup]=useState(false);
    //console.log(isSingup)
    const [inputs, setInputs]=useState({
       name:"", email:"", password:"" ,
    })
    const handleChange=(e)=>{
      setInputs((prevState)=>({
        ...prevState, 
        [e.target.name]:e.target.value
      }))
    }
    const handleSubmit=(e)=>{
      //e.preventDefault() this ()-> to submit the new http request
      e.preventDefault(); 
      console.log(inputs);
      navigate('/dashboard');
    }
    const resetState=(e)=>{
      setIsSingup(!isSingup);
      setInputs({name:'',email:'',password:''});
    }
    return (
      <div>
          <form onSubmit={handleSubmit}>
              <Box display="flex" flexDirection={"column"} maxWidth={400} 
              alignItems="center" justifyContent={'center'} margin='auto' marginTop={5} padding={3}
              borderRadius={5} boxShadow={'5px 5px 10px #ccc'} sx={{":hover":{
                  boxShadow:'10px 10px 20px #ccc'  }}}>
            
                {/* Here in the below line we add the condition for Login page and signup page Greetings to display and rendered accordingly   */}
                  <Typography variant='h3' padding={3} textAlign="center">{isSingup ? "Get Ready Prodaptians" : "Bonjur Prodaptians"}</Typography> 

                        {/* Here in the below line we add the condition for Login page and signup page Fields to display and rendered accordingly   */}
                      {isSingup && <TextField  value={inputs.name} type={'text'} name="name" 
                      onChange={handleChange}
                      variant='outlined' placeholder='Name' margin='normal'></TextField>}

                        <TextField type={'email'} name="email" value={inputs.email} 
                        onChange={handleChange}
                        variant='outlined' placeholder='Email' margin='normal'></TextField>

                        <TextField type={'password'} name="password" value={inputs.password}
                        onChange={handleChange}
                        variant='outlined' placeholder='Password' margin='normal'></TextField>
                      
                      {/* Here in the below line we add the condition for Login page and signup page Buttons to display and rendered accordingly   */}
                      <Button endIcon={isSingup ? <HowToRegIcon/>: <LoginIcon/>}
                      type="submit" sx={{marginTop:3, borderRadius:3}}  variant='contained' color='warning'>{isSingup? "New Register" : "Login "}</Button>

                      {/* Here in the below line we add the condition for Login page and signup page text to display and rendered accordingly */}
                      <Button endIcon={isSingup ? <LoginIcon/>:<HowToRegIcon/>}
                      onClick={resetState}sx={{marginTop:3, borderRadius:3}} > Change To {isSingup?"Login":"Signup"}</Button>
              </Box>
          </form>
      </div>
    )
  } 

export default Login
