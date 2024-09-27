# Reusuable Component

The following is an example of a reusuable component. A component that is configurable via its properties and can be used anywhere in the application. This provides flexibility without code duplication.

Example:

```typescript
import type { StaticImageData } from 'next/image';
import Image from 'next/image';

interface HeroProps {
  imgData: StaticImageData;
  imgAlt: string;
  title: string;
}

export default function Hero(props: HeroProps) {
  return (
    <div className="relative h-screen">
      <div className="absolute -z-10 inset-0">
        <Image
          src={props.imgData}
          alt={props.imgAlt}
          fill
          style={{ objectFit: 'cover' }}
        />
        <div className="absolute inset-0 bg-gradient-to-r from-slate-900" />
      </div>
      <div className="pt-48 flex justify-center items-center">
        <h1 className="text-white text-6xl">{props.title}</h1>
      </div>
    </div>
  );
}
```

The above is a Hero component that takes as properties an image, the alt text for the image and a title and displays these to fill the entire container that the component is in.

Example usage:

```typescript
import homeImg from 'public/home.jpg';
import Hero from '@/components/hero';

export default function Home() {
  return (
    <Hero
      imgData={homeImg}
      imgAlt="car factory"
      title="Professional Cloud Hosting"
    />
  );
}
```
