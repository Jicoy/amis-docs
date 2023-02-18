# Backend folder structure

```shell
app/
├── Http/
│   ├── Controllers/
│   │   ├── Admin/
│   │   │   ├── AdminMaController.php
│   │   │   ├── AdminMaTxnController.php
│   │   │   └── MentorAssignmentController.php
│   │   ├── Faculty/
│   │   │   ├── AdminMaController.php
│   │   │   ├── AdminMaTxnController.php
│   │   │   └── MentorAssignmentController.php
│   │   ├── Student/
│   │   │   ├── StudentMaController.php
│   │   │   └── StudentMaTxnController.php
│   │   ├── FacultiesController.php
│   │   ├── MentorAssignmentController.php
│   │   ├── MentorController.php
│   │   └── MentorRoleController.php
│   └── Requests/
│       └── MentorAssignment/
│           └── SubmitRequest.php
└── Services/
...

database/
├── migrations/
│   ├── 2022_06_26_163036_create_save_mentors_table.php
│   ├── 2022_06_26_172750_create_mentors_table.php
│   ├── 2022_11_02_182928_create_mentor_roles_table.php
│   ├── 2023_01_28_104822_create_mas_table.php
└── └── 2023_01_28_104850_create_matxns_table.php

...

routes/
└── api.php
```

<a name="adminmacontroller"></a>
## AdminMaController

'Ma' stands for MentorAssignment inside this controller:



```php

public function index(Request $request, TagProcessor $tagProcessor)
{
    $request->merge(['access_permission' => 'tags']);
    $ma = Ma::filter($request, $tagProcessor);
    
    if($request->has('items')) {
        $ma = $ma->paginate($request->items);
    } else {
        $ma = $ma->get();
    }
    
    return response()->json([
        'ma' => $ma
    ],200);
}

```



# Frontend folder structure

```shell
components/
├── mentor-assignments/
│   ├── MentorAssignmentTable.vue
│   ├── StudentActiveMentor.vue
│   └── StudentAddMentor.vue
├── GenericDrawer.vue  
├── GenericTable.vue  
└── StudentInfo.vue  
pages/
├── admin/
│   ├── mentor-assginment-college.vue
│   └── mentor-assginment-unit.vue
├── faculty/
│   ├── mentor-assignments.vue
│   └── my-advisees.vue
├── student/
└── └── mentor-assignments.vue
store/
├── admin/
│   └── mentorAssignment.js
├── faculty/
│   └── mentorAssignment.js
├── student/
└── └── mentorAssignment.js
```

