---
layout: post
title:  "React Router Auth and Typescript and Hooks"
date:   2021-02-28
categories: typescript
---

I recently transitioned a small app I built for my son to Typescript, to get some more practical "sandbox-y" experience with the language structures. The app is a tiny budgeting tool for kids (that _mostly_ helps him see how much money he's spent on Fortnite) and it only has a handful of views. It's a React front end and a Firebase backend.

The app, like many React projects, uses the [React Router](https://reactrouter.com/) library for routing and navigation. When attempting to get a simple authentication flow in place, I ran into a couple of interesting challenges that I thought I'd note down here, for future me.

You can see the full auth flow demonstrated all in one place here in [codesandbox](https://codesandbox.io/s/typscript-router-sandbox-otfln?file=/src/App.tsx). And you can see the code broken down into separate components and integrated with Firebase Auth [in the Github repo here](https://github.com/switzerb/money-blender/tree/master/src).

In the React Router docs, they give a simple example of an auth redirect flow: [React Router Auth Workflow](https://reactrouter.com/web/example/auth-workflow). It's using Context and Hooks to create an `Auth Provider` component to easily be able to access authentication information and to direct navigational business logic. The most interesting part for me was this wrapper component, that wraps the `Route` library component with an auth check, that will automatically redirect to login if the user is not authenticated. See how the render prop is conditional based on the presence of the user?

{% highlight javascript linenos %}
{% raw %}
function PrivateRoute({ children, ...rest }) {
  let auth = useAuth();
  return (
    <Route
      {...rest}
      render={({ location }) =>
        auth.user ? (
          children
        ) : (
          <Redirect 
              to={{ 
                pathname: "/login", 
                state: { from: location }
              }} 
          />
        )
      }
    />
  );
}
{% endraw %}
{% endhighlight %}

Below, you can see the wrapped component in Typescript, with just a few changes. I've defined a type for the props, to make it clearer what is happening here and used a syntax to render the component child that I feel is more intuitive for me. I am not completely happy with the spread here, since I think it introduces a lot of wiggle into the component, but since it was a personal project I just let it sit as-is.

{% highlight typescript linenos %}
{% raw %}
type PrivateRouteProps = {
  path: string;
  exact?: boolean;
  component: React.FC<RouteComponentProps>;
};

function PrivateRoute({ component: Component, ...rest }: PrivateRouteProps) {
  let auth = useAuth();
  return (
    <Route
      {...rest}
      render={(props: RouteComponentProps) =>
        auth && auth.user ? (
          <Component {...props} />
        ) : (
          <Redirect 
              to={{ 
                pathname: "/login", 
                state: { prevPath: rest.path }
              }} />
        )
      }
    />
  );
}
{% endraw %}
{% endhighlight %}
