# Lab 12: Hiding and Showing Components

## Objectives

- [ ] Add state to a component
- [ ] Hide and show a component

## Steps

### Add state to a component

1. Add a `state` property `editingProject` to hold the project currently being edited.

   #### src\projects\ProjectList.js

   ```diff
   import React from 'react';
   import { Project } from './Project';
   import ProjectCard from './ProjectCard';
   import ProjectForm from './ProjectForm';

    class ProjectList extends React.Component{
   +  state = {
   +    editingProject: {}
   +  };
      ...
    }
   ```

1. Update `handleEdit` to set the `editingProject` property in `state`.

   #### src\projects\ProjectList.js

   ```diff
   class ProjectList extends React.Component {
     state = {
       editingProject: {}
     };
     handleEdit = (project) => {
   -   console.log(project);
   +   this.setState({ editingProject: project });
     };
   }
   ```

### Hide and show a component

1. Modify the `render` method to show the `ProjectCard` for every `project` except for the currently `editingProject` which should display the `ProjectForm`.

   > Hint: create a variable to store the item (Card or Form) to make this more readable.

   ```js
   let item;
   ```

   > Refer to your manual **Chapter 12: Hiding and Showing Components** or the solution to the lab to get the correct syntax.

   #### src\projects\ProjectList.js

   ```diff
   import React from 'react';
   import PropTypes from 'prop-types';
   import { Project } from './Project';
   import ProjectCard from './ProjectCard';
   import ProjectForm from './ProjectForm';

   class ProjectList extends React.Component {
     state = {
       editingProject: {}
     };
     handleEdit = project => {
       this.setState({ editingProject: project });
     };
     render() {
       const { projects } = this.props;

   +    let item;
   +    const items = projects.map(project => {
   +      if (project !== this.state.editingProject) {
   +        item = (
   +          <div key={project.id} className="cols-sm">
   +            <ProjectCard
   +              project={project}
   +              onEdit={() => {
   +                this.handleEdit(project);
   +              }}
   +            ></ProjectCard>
   +          </div>
   +        );
   +      } else {
   +        item = (
   +          <div key={project.id} className="cols-sm">
   +            <ProjectForm></ProjectForm>
   +          </div>
   +        );
   +      }
   +      return item;
   +    });

       return <div className="row">{items}</div>;
     }
   }

   ProjectList.propTypes = {
     projects: PropTypes.arrayOf(PropTypes.instanceOf(Project)).isRequired
   };

   export default ProjectList;
   ```

2. **Verify** the application is **working** by _following these steps_:

   1. **Open** the application in your browser and refresh the page.
   2. **Click** the **edit** button for a project.
   3. **Verify** the `<ProjectCard />` is removed and replaced by the `<ProjectForm/>`.

      > The `<ProjectForm/>` will be empty at this point. We will fill in the data in a future lab.

      > You can do click another edit button on another `ProjectCard` and that card will change to a form. At this point, you may notice that the cancel button doesn't work. We will get that working in the next lab.

   ![image](https://user-images.githubusercontent.com/1474579/64925618-6b473700-d7c1-11e9-9cbc-f2899bc1968a.png)

---

### &#10004; You have completed Lab 12

<br/>
<br/>

> If you are curious, below is an alternate syntax for the hide and show a component part of the lab.
> Please **DO NOT UPDATE the code** as shown below -- it is simply here to illustrate the alternative syntax.

#### src\projects\ProjectList.js

```js

class ProjectList extends React.Component{
...
  render() {
    const { projects } = this.props;
    const items = projects.map(project => (
      <div key={project.id} className="cols-sm">
        {project !== this.state.editingProject ? (
          <div key={project.id} className="cols-sm">
            <ProjectCard
              project={project}
              onEdit={(project: Project) => {
                this.handleEdit(project);
              }}
            ></ProjectCard>
          </div>
        ) : (
          <div key={project.id} className="cols-sm">
            <ProjectForm></ProjectForm>
          </div>
        )}
      </div>
    ));
    return <div className="row">{items}</div>;
  }
}
```
