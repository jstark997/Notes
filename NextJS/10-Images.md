# Images

Static assets like images are placed in the public directory.

Images in a Next.js app can be rendered using the Image component.

The Image component:

- Renders local (stored with your project) images
- Renders remote (hosted somewhere else) images
- Will resize the image for different device sizes
- Resized images are cached in the server

Exmaple:

```typescript
import Image from 'next/image';
import homeImg from './public/home.jpg';

export default function Home() {
  return (
    <div>
      Home Page
      <Image src={homeImg} alt="picture of a lovely house" />
    </div>
  );
}
```

The Image component has a style property that can be used to adjust how the image is presented on the screen. Also, the Image component can be wrapped within a \<div\> element the className property of which can be used to specify some CSS styling.

The Image component also prevents layout shifting when the image loads slowly compared to the any surrounding text.

Options for image sizing:

- For a locally sourced image, the Image component uses the dimensions of the image by default
- Directly assign a height and width to the Image component using the height and width properties
- Use the 'fill' property of the Image component to expand the image to fill the parent element
