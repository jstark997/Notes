# Image Component Details

Next.js has an Image Optimization API that will automatically generate a set of images based on a source image and the srcset for those images. Next.js has a built in image loader that uses this API to create multiple URLs to request an image at different sizes which then becomes part of the srcset for that image.

The default image loader can optimize both local and remote images.

The Next.js Image component acts as a wrapper around the standard img element. By utilizing the Image component in the following manner, you can take advantage of Next.js’s image optimization capabilities. For an Image component used like this:

The Next.js Image component is a wrapper around the standard img element.

**Example**:

From an `Image` component such as the following:

```typescript
import Image from "next/image";

const MyComponent = () => (
  <Image
    src="/assets/nature-mountains.jpg"
    width={800}
    height={600}
    alt="Example Image"
  />
);
```

Next.js will generate the following `img` element:

```html
<img
  alt="Example Image"
  loading="lazy"
  width="800"
  height="600"
  decoding="async"
  data-nimg="1"
  style="color:transparent"
  srcset="
    /_next/image?url=%2Fassets%2Fnature-mountains.jpg&amp;w=828&amp;q=75  1x,
    /_next/image?url=%2Fassets%2Fnature-mountains.jpg&amp;w=1920&amp;q=75 2x
  "
  src="/_next/image?url=%2Fassets%2Fnature-mountains.jpg&amp;w=1920&amp;q=75"
/>
```
