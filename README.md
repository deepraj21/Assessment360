### Assessment360


### To be deployed live at : pythonanywhere.com

## Installation

1. **Clone the repository:**

   ```bash
   git clone https://github.com/Abhishek-Mallick/Assessment360.git
   cd Assessment360
   ```

2. **Set up MongoDB:**

   - Make sure MongoDB is installed and running on your machine or you can use my mongo db test url.
   - Create a `.env` file in the root directory with the following content:

     ```env
     MONGO_URL=mongodb://localhost:27017/Assessment360
     ```

     or

     ```
     MONGO_URL = "mongodb+srv://<username>:<password>@<clusterName>.<clusterID>.mongodb.net/?retryWrites=true&w=majority"
     ```

3. **Set up Enviornment Variables**
   - Create a  `.env` file on the base of the directory. On the `.env` file add the variables:
     ```
     MONGO_URL=""
     MAIL_SERVER="" (eg: smtp.gmail.com)
     MAIL_PORT=465
     MAIL_USERNAME=""mallickabhishek.64@gmail.com""
     MAIL_PASSWORD=""
     O_AUTH_CLIENT_ID=""
     ```

## Running the Application

1. **Setting up Virtual Enviornment (venv)**
```bash
 python -m venv venv
 ./venv/Scripts/activate
```
2. **Installing dependencies**
```bash
 pip install -r "requirements.txt"
```

3. **Creating super_user**
```bash
 python manage.py createsuperuser"
```
4. **Running the Application**
```bash
 python manage.py makemigrations
 python manage.py migrate 
 python manage.py runserver
```
## Endpoints

### Admin (super_user)

| Method | Endpoint             | Description         | Request Body                  | Response Body              |
| ------ | -------------------- | ------------------- | ----------------------------- | -------------------------- |
| `POST` | `/admin/register` | Register a new Admin | JSON: {name,priority} | JSON: {token,user,admin_id} |
| `GET`  | `/admin/:id` | Get Admin details    | NULL                          | JSON: {admin}               |

### Tasks

| Method   | Endpoint             | Description                | Query                                   | Headers                         | Request Body                       | Response Body                                |
| -------- | -------------------- | -------------------------- | --------------------------------------- | ------------------------------- | ---------------------------------- | -------------------------------------------- |
| `GET`    | `/student/assignment`         | Get all Assignemnts of the Student of all courses  | priority, status, due_date, page, limit | Authorization: Bearer JWT token | N/A                                | JSON: {docs,totalDocs,page,limit,totalPages} |
| `POST`   | `/student/update-assign`         | Create a new Assignment for Student under a Course | N/A                                     | Authorization: Bearer JWT token | JSON: {title,description,due_date,pdf_location} | JSON: {task,message}                         |
| `PATCH`  | `/api/task/:task_id` | Update task for Student       | N/A                                     | Authorization: student_id | JSON: {status,due_date}            | JSON: {task,message}                         |
| `DELETE` | `/api/task/:task_id` | Soft Delete task for Student  | N/A                                     | Authorization: Bearer JWT token | N/A                                | JSON: {task,message}                         |

### Sub Tasks

| Method   | Endpoint                 | Description                    | Query   | Headers                         | Request Body    | Response Body           |
| -------- | ------------------------ | ------------------------------ | ------- | ------------------------------- | --------------- | ----------------------- |
| `GET`    | `/discussion`         | Get all Discussion of the User  | task_id | Authorization: Bearer JWT token | N/A             | JSON: {subTasks}        |
| `POST`   | `/discussion/update`         | Create a new message for Student under Course | N/A     | Authorization: Bearer JWT token | JSON: {task_id} | JSON: {subTask,message} |
| `DELETE` | `/discussion/:diss_id` | Soft Delete Discussion for student  | N/A     | Authorization: Bearer JWT token | N/A             | JSON: {subTask,message} |

### Cron Jobs

| Name                     | Function Name     | Description                                                                                        |
| ------------------------ | ----------------- | -------------------------------------------------------------------------------------------------- |
| Task Priority            | `priorityCronJob` | It runs at midnight and checks the date and updates the priority accordingly                       |
| Call User if Task is Due | `priorityCronJob` | It runs after every hour and checks if task is due for some user calls the user and gives reminder |

## License

[MIT](https://choosealicense.com/licenses/mit/)
