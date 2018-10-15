# July 25th, 2018

## Dynamically Importing SVG

after many failed attempts I was able to dynamically import svg components by creating small components out of each of the svg's I wanted and then setting them in an object. I was then able to import them and use them like this...

```
import React from "react";

import currencies from "../../currency-icons";

type Props = {
  currency: String,
};

const Quote = ({ currency }: Props) => {
  const CurrencySvg = currencies[currency];
  return (
    <div className="quote">
      <CurrencySvg />
    </div>
  );
};

export default Quote;
```
