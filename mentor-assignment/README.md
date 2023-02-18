# Backend folder structure

```shell

app/
├── Http/
│   ├── Controllers/
│   │   ├── Admin/
│   │   │   ├── AdminMaController.php
│   │   │   ├── AdminMaTxnController.php
│   │   │   ├── MentorAssignmentController.php
│   │   ├── Faculty/
│   │   │   ├── AdminMaController.php
│   │   │   ├── AdminMaTxnController.php
│   │   │   ├── MentorAssignmentController.php
│   │   ├── Student/
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

