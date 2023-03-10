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


## FacultiesController
This conntroller was designated for displaying `mentor_name` on this particular section. 
![alt text](https://github.com/Jicoy/amis-docs/blob/main/assets/mentor-assignment/Screenshot-2023-02-19-100353.png)
> **Note**  
> Two `models` in mind to remember, so that mentor/faculty will display; `Faculty` and `FacultyAppointment`.

The mentor/faculty should exists on both tables `faculties` and `faculty_appointments`

#### faculties
| faculty_id    |                 uuid                 | sais_id  |
| ------------- |:------------------------------------:| -------: |
| 5             | e4d3a590-9fe5-11ed-ae55-df4917c48579 | 19819812 |
| 6             | e4d295a0-9fe5-11ed-9e4e-436f538054c9 | 89089089 |

#### faculty_appointments
| id    |  faculty_id  |  homeunit  |  status   |
| ----- |:------------:| ----------:|----------:|
| 1     |      5       |    DX      |  ACTIVE   |
| 2     |      6       |    DX      |  ACTIVE   |

#### users
|       uuid                           |   sais_id   |   last_name   |   first_name   |   middle_name   |          email          |
| ------------------------------------ |:-----------:| -------------:|---------------:|-----------------|-------------------------|
| e4d3a590-9fe5-11ed-ae55-df4917c48579 |  19819812   |    TANEDO     |    JERVIS      |                 | jtanedo@up.edu.ph       |  
| e4d295a0-9fe5-11ed-9e4e-436f538054c9 |  89089089   | PAMULAKLAKIN  |      JEN       |                 | jpamulaklakin@up.edu.ph |

#### FacultiesController
```php
public function index(Request $request)
{
    $ma = Faculty::filter($request)->get();
    return response()->json(
        [
            'ma' => $ma,
        ], 200
    );
}
```
#### Faculty Model
```php
public function appointment() 
{
    return $this->belongsTo(FacultyAppointment::class, 'faculty_id', 'faculty_id');
}

public function uuid() 
{
    return $this->belongsTo(User::class, 'uuid', 'uuid');
}

public function scopeFilter($query, $filters) {
    if($filters->has('faculty_list')) {
        $query->with(['uuid', 'appointment']);
    }
    ...
}
```
#### store/mentorAssignment.js
```javascript
export const actions = {
    async getFaculty({commit }, payload) {
        commit('GET_DATA_REQUEST')
        try {
            const data = await this.$axios.$get(`/faculties`, {params: {faculty_list: true}})
            // console.log(data)
            await commit('GET_APPOINTMENT_SUCCESS', data.ma)
        } catch (error) {
            ...
        }
    }
}
```
Return `ACTIVE` faculty as shown below
```javascript
export const mutations = {
    GET_APPOINTMENT_SUCCESS(state, data) {
        if(data) {
            var facultyListData =  data.map((item) => {
                if(item.uuid != null) {
                    // let homeunit = item.appointment != null ? '('+item.appointment.homeunit+')' : ''
                    let homeunit = item.appointment != null ? item.appointment.homeunit : ''
                    let faculty_status = item.appointment != null ? item.appointment.status : ''
                    var temp = Object.assign({
                        home_unit: homeunit,
                        faculty_id: item.faculty_id,
                        faculty_status: faculty_status,
                        faculty_name: item.uuid.last_name+' '+item.uuid.first_name+' '+ homeunit
                    }) 

                    if(temp.faculty_status === 'ACTIVE') {
                        return temp
                    }
                }
            })
            state.facultyList = facultyListData
        }
    }
}
```
> For future developer: Improve this approach. 






## MentorRoleController
This controller was designated for displaying `mentor_role` on this particular section  
![alt text](https://github.com/Jicoy/amis-docs/blob/main/assets/mentor-assignment/Screenshot-2023-02-18-223518.png)

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
This is how it fetch data from `backend`
> **Note**  
> This `async getMentorRole` was already used by all roles and it is located at `store/mentorAssignment.js`

```javascript
export const actions = {
    async getMentorRole({commit}) {
        commit('GET_DATA_REQUEST')
        try {
            const data = await this.$axios.$get(`/mentor-roles`)
            await commit('GET_MENTOR_ROLES', data.ma)
        } catch (error) {
            ...
        }
    }
}
```

```javascript
export const mutations = {
    GET_MENTOR_ROLES(state, data) {
        let role = data.filter(x => x.id != 1)
        state.mentorRoles = role
    }
}
```
The data we get from backend was manipulated from mutations, we don't want to display `Temporary Adviser` which is equivalent from `1`, as show on `GET_MENTOR_ROLES`. Then send it back to `states`
```javascript
export const state = () => ({
    mentorRoles: {},
})
```
After we fetch data and manipulate from store, we can use it to our components which `components/mentor-assignments/StudentAddMentor.vue`
```javascript
export default() {
    computed: {
          ...mapState({
              mentorRoles: state => state.mentorAssignment.mentorRoles
          }),
    }
}
```

```html
<td>
    <select :value="record.mentor_role" @change="(value) => update('mentor_role', record.id, value)" class="text-md border border-gray-400 rounded p-1">
    <option v-for="(mentorRole, mentorRoleIndex) in mentorRoles"  :key="mentorRoleIndex" :value="mentorRole.id">
        {{ mentorRole.titles }}
    </option>
    </select>
</td>
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
└── mentorAssignment.js
```

