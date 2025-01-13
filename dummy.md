import { useState } from 'react'
import './App.css'
import 'bootstrap/dist/css/bootstrap.min.css';
import HeaderCompnent from './components/HeaderComponent'
import FooterComponent from './components/FooterComponent'
import ListEmployeeComponent from './components/ListEmployeeComponent'
import EmployeeComponent from './components/EmployeeComponent'
import {BrowserRouter, Routes, Route, Router} from 'react-router-dom'

function App(){

  return(
    <>    
    <BrowserRouter>
    <HeaderCompnent/>
    <Routes>
      {/* //http://localhost:8080 */}
      <Route path='/' element={<ListEmployeeComponent/>}></Route>
      {/* //http://localhost:8080/employees */}
      <Route path='/employees' element={<ListEmployeeComponent/>}></Route>
      {/* //http://localhost:8080/add-employee */}
      <Route path='/add-employee' element={<EmployeeComponent/>}></Route>
      {/* //http://localhost:8080/edit-employee/1 */}
      <Route path='/edit-employee/:id' element={<EmployeeComponent/>}></Route>
    </Routes>
    <FooterComponent/>
    </BrowserRouter>    
    </>
  )
}

export default App
