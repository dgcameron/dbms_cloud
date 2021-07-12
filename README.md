# Case Study in the Use of DBMS_CLOUD (and Other Core Database Features) to Automate Loading Data into Autonomous Database (ADB)

Derrick Cameron
July, 2021

This documents a case study on work with a customer who had the following requirements (not strictly limited to dbms_cloud features):
- Automated load that processes data from daily and weekl csv files in Object Storage.
- Deletes the files once they are loaded.
- Log load processes.
- Audit updates to data with information about what was changed and when.
- Provide a dashboard that shows various daily, weekly and quarterly metrics current and prior measures, with logic that handles missing data points.

There are various ways to do this, and the availability of cloud services such as events (detection of file uploads to object storage) and functions (process to load the data), use of these services is often the preferred approach (see an example of this [here](https://github.com/oracle/oracle-functions-samples/tree/master/samples/oci-load-file-into-adw-python)).  This is certainly the case where processes are orchestrated and may involve other cloud services, however it does add some complexity, other dependent platforms and services, and the need to master python or java.

This documents an alternative approach that only requires the use of SQL (which is needed in any case) and no other platform.  It is scalable and can process millions of records in seconds even using a minimal 1-ocpu ADB shape, and also offers the full power of the SQL language to handle the processing of object storage files as though they were native database files.  To illustrate we'll use data from the sh schema that is available in every Oracle Database so others can replicate these steps in their own environment.

## **STEP 1:** Your Oracle Cloud Trial Account

### **Cloud Setup:**

- Create a new schema that will own objects (userid demo in this case):
```
<copy>
create user demo identfied by <password>;
grant dwrole, oml_developer, create table, create view to credit;
grant read, write on directory data_pump_dir to credit;
grant execute on dbms_cloud to credit;
alter user credit quota unlimited on data;
</copy>
```

- asdf

  ![](images/1/001.png " ")

