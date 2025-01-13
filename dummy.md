import { useState } from 'react';
import { BrowserRouter, Routes, Route } from 'react-router-dom';
import 'bootstrap/dist/css/bootstrap.min.css';
import './App.css';

// Components
import HeaderComponent from './components/HeaderComponent';
import FooterComponent from './components/FooterComponent';
import ListEmployeeComponent from './components/ListEmployeeComponent';
import EmployeeComponent from './components/EmployeeComponent';

function App() {
    return (
        <BrowserRouter>
            <div className="min-vh-100 d-flex flex-column">
                <HeaderComponent />
                
                <div className="flex-grow-1 container mt-4">
                    <Routes>
                        {/* Default route redirects to employees list */}
                        <Route path="/" element={<ListEmployeeComponent />} />
                        
                        {/* Employee list page */}
                        <Route path="/employees" element={<ListEmployeeComponent />} />
                        
                        {/* Add employee page */}
                        <Route path="/add-employee" element={<EmployeeComponent />} />
                        
                        {/* Edit employee page */}
                        <Route path="/edit-employee/:id" element={<EmployeeComponent />} />
                    </Routes>
                </div>

                <FooterComponent />
            </div>
        </BrowserRouter>
    );
}

export default App;