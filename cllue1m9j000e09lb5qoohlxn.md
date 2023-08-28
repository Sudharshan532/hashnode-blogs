---
title: "Build a REST API with Spring Boot"
seoTitle: "Build REST API with Spring Boot: Expert Guide for Success"
seoDescription: "Learn how to create a powerful REST API using Spring Boot. Step-by-step guide for building efficient APIs that drive seamless interactions. Start now!"
datePublished: Mon Aug 28 2023 04:37:30 GMT+0000 (Coordinated Universal Time)
cuid: cllue1m9j000e09lb5qoohlxn
slug: build-a-rest-api-with-spring-boot
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1692942712523/bc1954d1-6055-4e91-87f6-b856226add65.png
tags: spring, microservices, java, rest-api, springboot

---

In today's digital landscape, the demand for efficient and scalable web services is higher than ever. Whether you're developing a dynamic web application or a powerful mobile app, having a reliable and flexible architecture for your APIs is paramount. This is where the Spring Boot framework steps in, offering developers a streamlined way to create robust RESTful web services, yet effortlessly developed and maintained

## Introduction

In this tutorial, we'll walk through the process of creating a basic REST API using Spring Boot, complete with CRUD (Create, Read, Update, Delete) operations.

### Step 1: Setting up the project

Create your first Spring Boot project effortlessly using [Spring Initializr](https://start.spring.io/), a web-based tool that generates project structures with pre-configured settings.

* Choose Maven as a build tool and Java as a language
    
* Choose the Spring Boot version (Don't use Snapshot versions).
    
* Add dependencies Spring Web, Spring Data JPA, PostgreSQL Driver and Spring Dev Tools
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1692960613119/bfaef9c9-54cd-4b88-bd7f-61c1d0936254.png align="center")

Project Structure:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693193088390/e48d2451-7698-4721-af3c-6555ca83a8f2.png align="center")

**Note:** By default, spring initializer creates a class called RestApiApplication

```java
package com.employee.restapi.RestAPI;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class RestApiApplication {
	public static void main(String[] args) {
		SpringApplication.run(RestApiApplication.class, args);
	}
}
```

### Step 2: Database configuration in application.properties file

```java
spring.datasource.url=jdbc:postgresql://localhost:5432/postgres
spring.datasource.username=test
spring.datasource.password=test
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
logging.level.org.springframework=info
```

### Step 3: Creating the Entity

Create the entity(Employee) and use specific annotations.

```java
package com.employee.restapi.RestAPI.entity;
import jakarta.persistence.Column;
import jakarta.persistence.Entity;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.GenerationType;
import jakarta.persistence.Id;
import jakarta.persistence.Table;

@Entity
@Table(name="employee", schema = "company")
public class Employee {
	
	@Id
	@GeneratedValue(strategy = GenerationType.AUTO)
	private int id;
	
	@Column
	private String name;
	
	@Column
	private String email;

	public int getId() {
		return id;
	}

	public void setId(int id) {
		this.id = id;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public String getEmail() {
		return email;
	}

	public void setEmail(String email) {
		this.email = email;
	}

	public Employee(int id, String name, String email) {
		super();
		this.id = id;
		this.name = name;
		this.email = email;
	}
	
	public Employee() {
		
	}
}
```

### Step 4: Creating EmployeeTo class

Create an EmployeeTo class that resembles the Entity. We use EmployeeTo class as the data coming from the frontend. If we need to make any changes in the database we make changes to the EmployeeTo class and we set the data to entity.

```java
package com.employee.restapi.RestAPI.to;

public class EmployeeTo {
	
	private int id;
	
	private String name;
	
	private String email;

	public int getId() {
		return id;
	}

	public void setId(int id) {
		this.id = id;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public String getEmail() {
		return email;
	}

	public void setEmail(String email) {
		this.email = email;
	}

	public EmployeeTo(int id, String name, String email) {
		super();
		this.id = id;
		this.name = name;
		this.email = email;
	}
	
	public EmployeeTo() {
	
	}

	@Override
	public String toString() {
		return "EmployeeTo [id=" + id + ", name=" + name + ", email=" + email + "]";
	}
}
```

### Step 5: Setting up the JPA Repository

Create the EmployeeRepository which extends JpaRepository. Pass Employee entity and Primary key type as parameters.

```java
package com.employee.restapi.RestAPI.repository;
import org.springframework.data.jpa.repository.JpaRepository;
import com.employee.restapi.RestAPI.entity.Employee;

public interface EmployeeRepository extends JpaRepository<Employee, Integer>{

	public Employee findByName(String name);
}
```

### Step 6: Building the service

Create a service class that will handle the business logic. For example EmployeeService class.

```java
package com.employee.restapi.RestAPI.service;

import java.util.ArrayList;
import java.util.List;
import java.util.Optional;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import com.employee.restapi.RestAPI.entity.Employee;
import com.employee.restapi.RestAPI.repository.EmployeeRepository;
import com.employee.restapi.RestAPI.to.EmployeeTo;
import jakarta.persistence.EntityNotFoundException;

@Service
public class EmployeeService {
	
	@Autowired
	private EmployeeRepository employeeRepository;

	public List<EmployeeTo> getAllEmployees() {
		List<Employee> allEmployees = employeeRepository.findAll();
		List<EmployeeTo> allEmployeesTo = new ArrayList<>(); 
		
		for (Employee employee : allEmployees) {
			
			EmployeeTo empTo = new EmployeeTo();
			empTo.setId(employee.getId());
			empTo.setName(employee.getName());
			empTo.setEmail(employee.getEmail());
			
			allEmployeesTo.add(empTo);
		}
		return allEmployeesTo;
	}

	public EmployeeTo getEmployee(String EmpName) {
		Employee emp = employeeRepository.findByName(EmpName);
		EmployeeTo empTo = new EmployeeTo();
		empTo.setId(emp.getId());
		empTo.setName(emp.getName());
		empTo.setEmail(emp.getEmail());
		return empTo;
	}

	public void createEmployee(EmployeeTo employeeTo) {
		
		Employee employee = new Employee();
		employee.setId(employeeTo.getId());
		employee.setName(employeeTo.getName());
		employee.setEmail(employeeTo.getEmail());
		employeeRepository.save(employee);
	}

	public void deleteEmployee(String name) {
		
		Employee employee = employeeRepository.findByName(name);
		if(employee!=null) {
			employeeRepository.delete(employee);
		}else {
			throw new EntityNotFoundException("Employee not found with the name "+employee);
		}
	}
	
	
	public EmployeeTo updateEmployee(EmployeeTo employeeTo) {
		
		Optional<Employee> employeeOptional = employeeRepository.findById(employeeTo.getId());
		if(!employeeOptional.isEmpty()) {
			Employee employee = employeeOptional.get();
			employee.setId(employeeTo.getId());
			employee.setName(employeeTo.getName());
			employee.setEmail(employeeTo.getEmail());
			
			Employee emp = employeeRepository.save(employee);
			
			EmployeeTo empTo = new EmployeeTo();
			empTo.setId(emp.getId());
			empTo.setName(emp.getName());
			empTo.setEmail(emp.getEmail());
			
			return empTo;
		}
		return null;
	}
	
}
```

### Step 7: Implementing the RestController

Create a REST controller to handle incoming HTTP requests and interact with the service. For example, **'EmployeeController'.**

```java
package com.employee.restapi.RestAPI.controller;

import java.util.List;

import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.DeleteMapping;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.PutMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import com.employee.restapi.RestAPI.service.EmployeeService;
import com.employee.restapi.RestAPI.to.EmployeeTo;

@RequestMapping("employee-api")
@RestController
public class EmployeeController {

	private EmployeeService employeeService;
	
	public EmployeeController(EmployeeService employeeService) {
		super();
		this.employeeService = employeeService;
	}

	@GetMapping("/")
	public ResponseEntity<List<EmployeeTo>> getAllEmployees(){
		
		List<EmployeeTo> empList = employeeService.getAllEmployees();
		return new ResponseEntity<List<EmployeeTo>>(empList,HttpStatus.OK);
	}
	
	@GetMapping("/{EmpName}")
	public ResponseEntity<EmployeeTo> getEmployeeByname(@PathVariable String EmpName){
		
		EmployeeTo empTo = employeeService.getEmployee(EmpName);
		return new ResponseEntity<EmployeeTo>(empTo,HttpStatus.OK);
	}
	
	@PostMapping("/")
	public ResponseEntity<String> createEmployee(@RequestBody EmployeeTo employeeTo){
		employeeService.createEmployee(employeeTo);
		return new ResponseEntity<String>("Employee deatils submitted successfully",HttpStatus.CREATED);
	}
	
	@DeleteMapping("/{name}")
	public ResponseEntity<String> deleteEmployee(@PathVariable String name){
		employeeService.deleteEmployee(name);
		return new ResponseEntity<String>("Employee Deleted Successfully",HttpStatus.OK);
	}
	
	@PutMapping("/")
	public ResponseEntity<EmployeeTo> updateEmployee(@RequestBody EmployeeTo employeeTo){
		
		EmployeeTo empTo = employeeService.updateEmployee(employeeTo);
		return new ResponseEntity<EmployeeTo>(empTo,HttpStatus.OK);
	}
}
```

### Step 8: Testing the API

With the project set up and components in place, run your Spring Boot application. You can use tools like **Postman** to test your API endpoints for CRUD operations.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693196723298/7adfc804-95dd-4909-bdbc-2fb8da628cd9.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693196919001/19f55b78-e612-40e8-a0c8-f48888041f02.png align="center")

Congratulations! We've successfully created a basic REST API with CRUD operations using Spring Boot. This foundation can be expanded and customized to build more complex applications.

Remember to handle error scenarios, implement input validation, and secure your endpoints for production use. Happy coding!

%[https://twitter.com/SChennupalle/status/1696017833631838577?s=20]