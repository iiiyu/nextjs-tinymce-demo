# Self hosted TinyMCE 6.x in NextJS 12.x - Javascript version

## Create NextJS Project

```bash
yarn create next-app
```

<!-- more -->

This article is very subjective. If you do not feel comfortable viewing it, please close it as soon as possible.
If you think my article can help you, you can subscribe to this site by using [RSS](https://iiiyu.com/atom.xml).

## Install TinyMCE latest version

```bash
yarn add tinymce @tinymce/tinymce-react copy-webpack-plugin
```

## Copy TinyMCE files to public folder as self hosted

Copy static files(tinymce files) to public folder. Edit file `next.config.js`

```javascript
/** @type {import('next').NextConfig} */
const path = require("path");
const CopyPlugin = require("copy-webpack-plugin");

const nextConfig = {
  reactStrictMode: true,
  swcMinify: true,
  future: {
    webpack5: true,
  },
  webpack: (config, { buildId, dev, isServer, defaultLoaders, webpack }) => {
    config.plugins.push(
      new CopyPlugin({
        patterns: [
          {
            from: path.join(__dirname, "node_modules/tinymce"),
            to: path.join(__dirname, "public/assets/libs/tinymce"),
          },
        ],
      })
    );
    return config;
  },
  webpackDevMiddleware: (config) => {
    return config;
  },
};

module.exports = nextConfig;
```

## Create the editor component

Create the file `components/editor/CustomEditor.jsx`

```javascript
import { Editor } from "@tinymce/tinymce-react";
import React, { useRef } from "react";

export function CustomEditor(props) {
  const editorRef = useRef(null);
  const log = () => {
    if (editorRef.current) {
      console.log(editorRef.current.getContent());
    }
  };
  return (
    <Editor
      tinymceScriptSrc={"/assets/libs/tinymce/tinymce.min.js"}
      onInit={(evt, editor) => (editorRef.current = editor)}
      value={props.content}
      init={{
        height: 500,
        menubar: true,
        plugins: [
          "advlist",
          "autolink",
          "lists",
          "link",
          "image",
          "charmap",
          "preview",
          "anchor",
          "searchreplace",
          "visualblocks",
          "code",
          "fullscreen",
          "insertdatetime",
          "media",
          "table",
          "code",
          "help",
          "wordcount",
        ],
        toolbar:
          "undo redo | blocks | " +
          "bold italic forecolor | alignleft aligncenter " +
          "alignright alignjustify | bullist numlist outdent indent | " +
          "removeformat | help",
        content_style:
          "body { font-family:Helvetica,Arial,sans-serif; font-size:14px }",
      }}
      onEditorChange={props.handleOnEditorChange}
    />
  );
}
```

## Done

![](https://s2.loli.net/2022/08/28/Us1EyN2njPd6Zcl.png)

[Demo](https://github.com/iiiyu/nextjs-tinymce-demo)

## Referrals

Photo by <a href="https://unsplash.com/@mylifeasaryan_?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Aryan Dhiman</a> on <a href="https://unsplash.com/s/photos/keyboard?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>

[NextJs- React - Self hosted TinyMCE](https://gist.github.com/zhangshine/00607fa3fe89195ef0a0e88983174b37#file-tinymce-react-nextjs-md)

[Hosting the TinyMCE package with the React framework](https://www.tiny.cloud/docs/tinymce/6/react-pm-host/)
