```tsx

import { FlashList } from "@shopify/flash-list";
import { Children, ForwardedRef, forwardRef } from "react";

type LazyScrollViewProps = React.ComponentProps<typeof FlashList> & {
  estimatedScrollViewSize?: { height: number; width: number };
  estimatedItemSize: number;
  children: Array<any>;
};

export const LazyScrollView = forwardRef(
  (props: LazyScrollViewProps, ref: ForwardedRef<FlashList<unknown[]>>) => {
    const { children, estimatedScrollViewSize, estimatedItemSize } = props;

    const data = Array.isArray(children)
      ? children
      : Children.toArray(children);

    // No specific reason for 2000, just a big number. Roughly 2x the screen size
    const drawDistance = 2000;

    if (estimatedItemSize === undefined) {
      console.warn(
        "LazyScrollView: 'estimatedItemSize' is missing, check FlashList warning for a recommended value."
      );
    }

    return (
      <FlashList
        ref={ref}
        {...props}
        data={data}
        renderItem={({ item }) => item}
        getItemType={(_, index) => {
          // Disables recycling of items by assigning a unique type to each item
          return index;
        }}
        estimatedListSize={estimatedScrollViewSize}
        drawDistance={drawDistance}
      />
    );
  }
);

```
