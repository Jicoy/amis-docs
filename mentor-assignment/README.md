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

<!-- ## AdminMaController.php

'Ma' stands for MentorAssignment; inside this controller we have this `index` and `update` :

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
``` -->


## MentorAssignmentController

## MentorRoleController
This controller was designated for displaying `mentor_role` in this particular section  

![alt text](https://github.com/Jicoy/amis-docs/blob/main/assets/mentor-assignment/Screenshot%202023-02-18%20223518.png)

`backend code`

```php
public function index()
{
    $ma = MentorRole::get();
    return response()->json(
        [
            'ma' => $ma,
        ], 200
    );
}
```
This is how it fetch from `backend` to `frontend`

`actions`
```javascript
async getMentorRole({commit}) {
    commit('GET_DATA_REQUEST')
    try {
        const data = await this.$axios.$get(`/mentor-roles`,)
        await commit('GET_MENTOR_ROLES', data.ma)
    } catch (error) {
        ...
    }
},
```
`mutations`
```javascript
GET_MENTOR_ROLES(state, data) {
    let role = data.filter(x => x.id != 1)
    state.mentorRoles = role
}, 
```
Data was manipulated from mutations, we don't want to display `Temporary Adviser` which is equivalent from `1`, as show on `GET_MENTOR_ROLES`


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

