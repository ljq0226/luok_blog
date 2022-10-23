

# Motion

## Animation
### test




## KeyFrames

![](https://obs-pic-1309372570.cos.ap-chongqing.myqcloud.com/keyframes.gif)

```tsx
import React from 'react'
import { motion } from 'framer-motion'
const KeyFrames = () => {
  return (
    <div>
      <motion.div
        initial={{}}
        animate={{ scale: [1, 2, 2, 1, 1], rotate: [0, 0, 180, 180, 0], borderRadius: ["0%", "0%", "50%", "50%", "0%"] }}
        transition={{
          duration: 2, ease: "easeInOut",
          times: [0, 0.2, 0.5, 0.8, 1],
          repeat: Infinity,
          repeatDelay: 1
        }}
        className='bg-red-500 w-52 h-52'
      >
      </motion.div>
    </div>
  )
}
export default KeyFrames
```



