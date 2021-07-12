# Case Study in the Use of DBMS_CLOUD (and Other Core Database Features) to Automate Loading Data into Autonomous Database (ADB)

Derrick Cameron
July, 2021

This documents a case study in work with a customer that had the following requirements:
- Departments deposit csv files with daily and weekly data into Object Storage.
- An automated process loads these files into oracle tables and deletes the files once loaded.
- Load processes are logged.
- Updates to the data are audited with information about what was changed and when.
- Data feeds a dashboard that provides various daily, weekly and quarterly metrics current and prior measures, with logic that handles missing data points.

There are various ways to do this, and the availability of cloud services such as events (detection of file uploads to object storage) and functions (process to load the data) often is the preferred approach (see an example of this [here](https://github.com/oracle/oracle-functions-samples/tree/master/samples/oci-load-file-into-adw-python)).  This is certainly the case where processes are orchestrated and may involve other cloud services, however it does add some complexity, other dependent platforms and services, and the need to master python or java.

This documents an alternative approach that only requires the use of SQL (which is needed in any case) and no other platform.  It is scalable and can process millions of records in seconds even using a minimal 1-ocpu ADB shape, and also offers the full power of the SQL language to handle the processing of object storage files as though they were native database files.

### **Options in Recommended Order (Depends Partly on Use Case):**

- [**Oracle Data Integrator (ODI) with BI Cloud Connector (BICC)**](odi.md) (recommended):
    - No data volume limits.
    - Oracle Data Integrator (ODI).  ODI can manage the BICC source and DBCS/ADW targets, and orchestrate the movement of data from BICC to Object Storage and from there to your target.  This is the recommended tool.
