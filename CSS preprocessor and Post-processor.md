## **CSS Preprocessor:**

CSS preprocessor compile the code which is written with a special syntax, Then processor will create a Pure CSS file, which can then be reference to the main HTML document.

Generally, we recommend that you don't reuse same CSS classes across different componenets.

`For Example:` instead of using a `.Button` CSS class in `<AcceptButton>` and `<RejectButton>` components, we recommend creating a `<Button>` component with it own `.Button` styles, that both `<AcceptButton>` and `<RejectButton>` can render.

Following this rule often makes CSS preprocessor less useful, as features like `mixins` and `nesting` are replaced by component compositiion.

To use SCSS:

```
npm install sass
```

## **CSS Post-processor:**

Post-processor happens after we already produced the plain CSS, and want to extend it further through automation. This can include `extending class selectors`, or `auto-appending prefixes` for certain properties.

![alt text](./images/pre%20and%20post%20CSS.PNG)

This project setup minifies your CSS and adds vendor prefixes to it automatically through `Autoprefixer` so we don't need to worry about it.

Support for new CSS features like the `all property`, `break properties`, `custom property` and `media query range` are automatically polyfilled to add and support for older browsers.

You can customize your target support browsers by adjusting the browserslist key in package.json according to the `Browserslist specification`.

**For Example:**

```CSS
.App {
  display: flex;
  flex-direction: row;
  align-items: center;
}
```

**becomes this:**

```CSS
.App {
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-orient: horizontal;
  -webkit-box-direction: normal;
  -ms-flex-direction: row;
  flex-direction: row;
  -webkit-box-align: center;
  -ms-flex-align: center;
  align-items: center;
}
```
