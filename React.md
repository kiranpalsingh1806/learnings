- [1. Custom Button in React](#1-custom-button-in-react)

## 1. Custom Button in React

```js
import { styled } from '@mui/system';

// Custom styled button
const CustomButton = styled(Button)({
    backgroundColor: '#4CAF50',
    borderRadius: '20px',
    color: 'white',
    padding: '5px',
    width: '50%',
    fontWeight: 'bold',
    '&:hover': {
        backgroundColor: '#45a049',
    },
    fontSize: '14px',
});

```