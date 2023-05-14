1.django-admin startapp emp_app<App name>

---------------------------------------------------

2. setting.py :add emp_app to Installed_apps
   Mention database property in setting.py i.e create data base 
--------------------
DATABASES = {
    'default': {
        'ENGINE'  : 'django.db.backends.mysql', # <-- UPDATED line 
        'NAME'    : 'employeedb',                 # <-- UPDATED line <Databse name>
        'USER'    : 'root',                     # <-- UPDATED line
        'PASSWORD': 'root',              # <-- UPDATED line
        'HOST'    : 'localhost',                # <-- UPDATED line
        'PORT'    : '3306',
    }
}

-------------------------------------------------------------
3.Create models in emp_app:
create class: Department,Role,Employee
e.g

class Employee(models.Model):
    first_name = models.CharField(max_length=100, null=False)
    last_name = models.CharField(max_length=100)
    dept = models.ForeignKey(Department, on_delete=models.CASCADE)
    salary = models.IntegerField(default=0)
    bonus = models.IntegerField(default=0)
    role = models.ForeignKey(Role, on_delete=models.CASCADE)
    phone = models.IntegerField(default=0)
    hire_date = models.DateField()


---------------------------------------------------------------------------------------------

4. write python code in views.py mentionaing its functionality <what to be displayed when user
 clicks on the link >

e.g.
def filter_emp(request):
    if request.method == 'POST':
        name = request.POST['name']
        dept = request.POST['dept']
        role = request.POST['role']
        emps = Employee.objects.all()
        if name:
            emps = emps.filter(Q(first_name__icontains = name) | Q(last_name__icontains = name))
        if dept:
            emps = emps.filter(dept__name__icontains = dept)
        if role:
            emps = emps.filter(role__name__icontains = role)

        context = {
            'emps': emps
        }
        return render(request, 'view_all_emp.html', context)

    elif request.method == 'GET':
        return render(request, 'filter_emp.html')
    else:
        return HttpResponse('An Exception Occurred')




To extract tables in to sql directly use
: python manage.py migrate;


-------------------------------------------------------------------------------------------------------------

5. All the front end web page needs to be written in template folder
that contains the html pages.
e.g


<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>View All Employee</title>
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-1BmE4kWBq78iYhFldvKuhfTAU6auU8tT94WrHftjDbrCEXSU1oBoqyl2QvZ6jIW3" crossorigin="anonymous">

</head>
  <body>
  <div class="container">
          <h1>All Employee Details!</h1>
      <hr>
     <table class="table table-dark">
  <thead>
    <tr>
      <th scope="col">#</th>
      <th scope="col">First Name</th>
      <th scope="col">Last Name</th>
      <th scope="col">Salary</th>
      <th scope="col">Bonus</th>
      <th scope="col">Phone Number</th>
      <th scope="col">Role</th>
      <th scope="col">Department</th>
      <th scope="col">Location</th>
      <th scope="col">Hiredate</th>
    </tr>
  </thead>
         {% for emp in emps %}
  <tbody
-------------------------------------------------------------------------------------


python manage.py createsuperuser  : Creates admin/user with root user's right.

python manage.py runserver: runs the server