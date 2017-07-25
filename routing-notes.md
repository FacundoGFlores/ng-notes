# Routing Notes

### Introduction 

In traditionals websites before, each page was completely separate and distinct from others and every page was served independently from the server. When you requested `home.html`, the browser asked to the server for the `home.html` file, which the server would then return to the browser and then display. And if you went to `profile.html`, the browser would ask the server for that file and the file would be returned and then displayed and the *entire* page would be replaced.

In SPAs, you load a file in memory `index.html`, and then the rest of the pages are loaded via JavaScript. It means that only a few portions of `index.html` are replaced as you navigate from page to page. 

### Lazy loading

Core feature of Angular. Lazy loading allows your app to start up faster because it only needs to use the main App Module when the page initially loads. As you navigate between routes, it will load the additional modules as you define them in your routes.

Following the Angular's module pattern you can do something like:

```
// file: home.module.ts
const routes = [
  {path: '', component: }
]

@NgModule({
  imports: [CommonModule, RouterModule.forChild(routes)],
  declarations: [HomeComponent]
})
export default class HomeModule{}
```

Then in your `app.routes` instead of having `{path: '', component:HomeComponet}` you include the `home.module.ts`:

```
// file: app.routes.ts

const routes = {
  {path: '', loadChildren: 'app/home/home.module.ts'}
  //...
}
```

another thing is that in your `app.module.ts` you can remove the `HomeComponent` reference in `declarations`.

One of the advantages of using it is *cleaning* the `app.module.ts`, where now all the components are in the `app.routes.ts` file.

