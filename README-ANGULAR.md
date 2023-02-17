ng g —help

- Add a component in angular

```bash
ng g c nav --skip-tests
```

- Add a service in angular

```bash
ng g s \_services/account --skip-tests
```

In order to pass props from Parent to child component

- In the child component, we define a @Input annotation like this

```bash
@Input() usersFromHomeComponent: any;
```

- And in the parent component, we will pass like this

```bash
<app-register [usersFromHomeComponent]="users"></app-register>
```

- Add guard to the angular

```bash
ng g g _guards/auth --skip-tests
ng g g _guards/admin --skip-tests
```

- Add interceptor to the angular

```bash
ng g interceptor \_
```

```bash
ng g d _directives/has-role --skip-tests
```
