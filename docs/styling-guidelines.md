## Overview 

In this project, we have adopted a hybrid approach for styling by utilizing React Native’s StyleSheet API alongside NativeWind. 
This combination allows us to take advantage of the utility-first CSS approach while maintaining the benefits of React Native's native styling system. 

## Styling Methodology 

### JSX for UI Structure
   
- JSX allows us to define the structure of the UI using HTML-like syntax. It is the primary method for constructing the layout of components in React Native. 

### NativeWind for Utility-First Styling

- NativeWind is used to apply utility classes directly within the JSX code. 
    It provides an easy and fast way to apply common styles such as padding, margin, backgroundColor, and flex by adding class names directly to the components. 

- This approach is inspired by Tailwind CSS used in web development, where we apply predefined utility classes directly within the className attribute in JSX. 

### StyleSheet API for Custom and Complex Styles

- While NativeWind handles basic styling with utility classes, we use React Native’s StyleSheet API for more complex or dynamic styles that require JavaScript logic. 
