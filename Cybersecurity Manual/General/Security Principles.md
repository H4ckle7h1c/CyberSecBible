## CIA
-   **Confidentiality** ensures that only the intended persons or recipients can access the data.
-   **Integrity** aims to ensure that the data cannot be altered; moreover, we can detect any alteration if it occurs.
-   **Availability** aims to ensure that the system or service is available when needed.

## DAD (oposite to cia)
-   **Disclosure** is the opposite of confidentiality. In other words, disclosure of confidential data would be an attack on confidentiality.
-   **Alteration** is the opposite of Integrity. For example, the integrity of a cheque is indispensable.
-   **Destruction/Denial** is the opposite of Availability.

## Security Models
- Bell-LaPadula
	- Aims to protect *CONFIDENTIALITY*
	- No read-up, no-write down
	- Write-down & read-up
- Biba Model
	- Aims to protect *INTEGRITY*
	- No read-down
	- No write-up
	- Read-up & write-down
- Clark-Wilson Model 
	- Aims to achieve integrity by :
		- **Constrained Data Item (CDI)**: This refers to the data type whose integrity we want to preserve.
		-   **Unconstrained Data Item (UDI)**: This refers to all data types beyond CDI, such as user and system input.
		-   **Transformation Procedures (TPs)**: These procedures are programmed operations, such as read and write, and should maintain the integrity of CDIs.
		-   **Integrity Verification Procedures (IVPs)**: These procedures check and ensure the validity of CDIs.
-   Brewer and Nash model
-   Goguen-Meseguer model
-   Sutherland model
-   Graham-Denning model
-   Harrison-Ruzzo-Ullman model

## Defense in Depth

## ISO/IEC 19249
Architectural principles : 
1.  Domain Separation : ring for processor
2. Layering : OSI model
3. Encapsulation : exemple of methods to access to an object in poo
4. Redundancy : Raid 5
5. Virtualization : cloud 
Design principles :
1. Least Privilege
2. Attack Surface Minimisation
3. Centralized Parameter Validation
4. Centralized General Security Services
5. Preparing for Error and Exception Handling
###  Zero Trust versus Trust but Verify
##  Threat versus Risk
