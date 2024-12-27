
## Componente TypeScript

Usaremos el [(ngModel)] para enlazar el valor del input y el evento (ngModelChange) para detectar cambios. Además, capturaremos el evento de la tecla Enter para realizar una búsqueda con el término completo.

```
  
  import { Component } from '@angular/core';

  @Component({
    selector: 'app-user-filter',
    templateUrl: './user-filter.component.html',
    styleUrls: ['./user-filter.component.css']
  })
  export class UserFilterComponent {
    users = [
      { id: 1, name: 'John Doe', userId: 23 },
      { id: 2, name: 'Jane Smith', userId: 34 },
      { id: 3, name: 'Alice Johnson', userId: 53 },
      { id: 4, name: 'Bob Brown', userId: 90 },
    ];

    filterTerm: string = '';
    filteredUsers = [...this.users]; // Copia inicial para mostrar todos los usuarios.

    onFilterChange(): void {
      const term = this.filterTerm.toLowerCase();
      this.filteredUsers = this.users.filter(user => 
        user.name.toLowerCase().includes(term) || user.userId.toString().includes(term)
      );
    }

    onEnter(): void {
      this.onFilterChange(); // Realiza la búsqueda con el término completo.
    }
  }

```

## Plantilla HTML

```

  <div class="search-bar">
    <input
      type="text"
      [(ngModel)]="filterTerm"
      (ngModelChange)="onFilterChange()"
      (keydown.enter)="onEnter()"
      placeholder="Search by name or userId"
    />
  </div>

  <div class="user-list">
    <div *ngFor="let user of filteredUsers" class="user-card">
      <h3>{{ user.name }}</h3>
      <p>User ID: {{ user.userId }}</p>
    </div>
  </div>


```

## Explicación

    Two-Way Data Binding:
        Usamos [(ngModel)] para enlazar el valor del input a la variable filterTerm.

    Escucha de Cambios:
        Usamos (ngModelChange) para llamar al método onFilterChange cada vez que se ingresa o elimina una letra en el input.

    Búsqueda Flexible:
        En onFilterChange, convertimos el término de búsqueda y los datos relevantes (name y userId) a minúsculas para realizar una búsqueda insensible a mayúsculas/minúsculas.
        Filtramos el array original con base en si el nombre contiene el término o el userId coincide parcial o totalmente.

    Tecla Enter:
        Usamos (keydown.enter) para detectar cuando se presiona Enter, invocando la misma lógica de filtrado.

    Renderización Dinámica:
        Usamos *ngFor para recorrer el array filteredUsers, que se actualiza dinámicamente con los resultados del filtro.