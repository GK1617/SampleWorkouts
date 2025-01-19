import React, { useState } from 'react';
import { Box, Typography, TextField, Button } from '@mui/material';
import LoginIcon from '@mui/icons-material/Login';
import HowToRegIcon from '@mui/icons-material/HowToReg';
import { useNavigate } from 'react-router-dom';

const Login = () => {
    const navigate = useNavigate();
    const [isSignup, setIsSignup] = useState(false);
    const [inputs, setInputs] = useState({
        name: "",
        email: "",
        password: ""
    });

    const handleChange = (e) => {
        setInputs((prevState) => ({
            ...prevState,
            [e.target.name]: e.target.value
        }));
    };

    const handleSubmit = (e) => {
        e.preventDefault();
        // Validate inputs before navigation
        if (!inputs.email || !inputs.password || (isSignup && !inputs.name)) {
            alert('Please fill in all required fields');
            return;
        }
        console.log('Form submitted with data:', inputs);
        navigate('/dashboard');
    };

    const resetState = () => {
        setIsSignup(!isSignup);
        setInputs({ name: '', email: '', password: '' });
    };

    return (
        <div>
            <form onSubmit={handleSubmit}>
                <Box 
                    display="flex" 
                    flexDirection="column" 
                    maxWidth={400} 
                    alignItems="center" 
                    justifyContent="center" 
                    margin="auto" 
                    marginTop={5} 
                    padding={3}
                    borderRadius={5} 
                    boxShadow="5px 5px 10px #ccc"
                    sx={{
                        ":hover": {
                            boxShadow: '10px 10px 20px #ccc'
                        }
                    }}
                >
                    <Typography 
                        variant="h3" 
                        padding={3} 
                        textAlign="center"
                    >
                        {isSignup ? "Get Ready Prodaptians" : "Bonjour Prodaptians"}
                    </Typography>

                    {isSignup && (
                        <TextField
                            value={inputs.name}
                            type="text"
                            name="name"
                            onChange={handleChange}
                            variant="outlined"
                            placeholder="Name"
                            margin="normal"
                            fullWidth
                        />
                    )}

                    <TextField
                        type="email"
                        name="email"
                        value={inputs.email}
                        onChange={handleChange}
                        variant="outlined"
                        placeholder="Email"
                        margin="normal"
                        fullWidth
                        required
                    />

                    <TextField
                        type="password"
                        name="password"
                        value={inputs.password}
                        onChange={handleChange}
                        variant="outlined"
                        placeholder="Password"
                        margin="normal"
                        fullWidth
                        required
                    />

                    <Button
                        endIcon={isSignup ? <HowToRegIcon /> : <LoginIcon />}
                        type="submit"
                        sx={{ marginTop: 3, borderRadius: 3, width: '100%' }}
                        variant="contained"
                        color="warning"
                    >
                        {isSignup ? "New Register" : "Login"}
                    </Button>

                    <Button
                        endIcon={isSignup ? <LoginIcon /> : <HowToRegIcon />}
                        onClick={resetState}
                        sx={{ marginTop: 3, borderRadius: 3, width: '100%' }}
                    >
                        Change To {isSignup ? "Login" : "Signup"}
                    </Button>
                </Box>
            </form>
        </div>
    );
};

export default Login;
