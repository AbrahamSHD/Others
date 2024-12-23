
## app-routing.module.ts

```
  // app-routing.module.ts
  import { NgModule } from '@angular/core';
  import { RouterModule, Routes } from '@angular/router';
  import { AllSchoolsComponent } from './schools/all-schools/all-schools.component';
  import { SchoolDetailsComponent } from './schools/school-details/school-details.component';

  const routes: Routes = [
    { path: 'schools', component: AllSchoolsComponent },
    { path: 'schools/:nameSchool', component: SchoolDetailsComponent }, // Ruta con parámetro dinámico
    { path: '', redirectTo: '/schools', pathMatch: 'full' }, // Redirige a 'schools' por defecto
    { path: '**', redirectTo: '/schools' }, // Manejo de rutas no encontradas
  ];

  @NgModule({
    imports: [RouterModule.forRoot(routes)],
    exports: [RouterModule],
  })
  export class AppRoutingModule {}

```

## all-schools.component.html

```
  <!-- all-schools.component.html -->
  <ul>
    <li *ngFor="let school of schools">
      <a [routerLink]="['/schools', sanitizeName(school.name)]">
        {{ school.name }}
      </a>
    </li>
  </ul>

```

## all-schools.component.ts

```
  // all-schools.component.ts
  import { Component } from '@angular/core';

  @Component({
    selector: 'app-all-schools',
    templateUrl: './all-schools.component.html',
    styleUrls: ['./all-schools.component.css'],
  })
  export class AllSchoolsComponent {
    schools = [
      { name: 'Escuela Primaria Nacional', studentAdvisors: [], meetings: [] },
      { name: 'Instituto Técnico Superior', studentAdvisors: [], meetings: [] },
    ];

    sanitizeName(name: string): string {
      return name.trim().toLowerCase().replace(/\s+/g, '-');
    }
  }

```

## school-details.component.ts

```
  // school-details.component.ts
  import { Component, OnInit } from '@angular/core';
  import { ActivatedRoute } from '@angular/router';

  @Component({
    selector: 'app-school-details',
    templateUrl: './school-details.component.html',
    styleUrls: ['./school-details.component.css'],
  })
  export class SchoolDetailsComponent implements OnInit {
    sanitizedName: string | null = '';
    originalName: string = '';

    constructor(private route: ActivatedRoute) {}

    ngOnInit(): void {
      this.sanitizedName = this.route.snapshot.paramMap.get('nameSchool');
      if (this.sanitizedName) {
        this.originalName = this.sanitizedName.replace(/-/g, ' ');
      }
    }
  }

```

## school-details.component.html
```
  <!-- school-details.component.html -->
  <h1>Detalles de la Escuela</h1>
  <p>Nombre de la escuela: {{ originalName }}</p>

```