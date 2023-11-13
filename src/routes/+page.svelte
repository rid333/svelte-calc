<script lang="ts">
    const numbers: string[] = ['7', '8', '9', '4', '5', '6', '1', '2', '3', '0', '.'];
    const operations: string[] = ['/', 'x', '-', '+'];

    let disp: string = '0';
    let selectedOperation: string = '';
    let firstOperand: any = 0;
    let secondOperand: any = 0;

    const handleNumClick = (num: string) => {
        if (disp === '0' && num === '0') { return };
        if (num === '.' && disp.includes('.')) { return };

        if (selectedOperation === '') {
            if (num === '.' && disp === '0') { firstOperand = '0.'; return disp = firstOperand };
            firstOperand += num;
            return disp = disp === '0' ? firstOperand.slice(1,) : disp + num;
        } else {
            if (num === '.' && disp === '0') { secondOperand = '0.'; return disp = secondOperand };
            secondOperand += num;
            return disp = secondOperand.slice(1,);
        }
    }

    const handleDelete = () => {
        disp = disp.length === 1 ? '0' : disp.slice(0, -1);
        if (selectedOperation === '') {
            firstOperand = firstOperand.length === 1 ? '0' : firstOperand.slice(0, -1);
        } else {
            secondOperand = secondOperand.length === 1 ? '0' : secondOperand.slice(0, -1);
        }
    }

    const handlePlusMinus = () => {
        if (disp === '0') { return };
        if (selectedOperation === '') {
            firstOperand = firstOperand * -1;
            return disp = firstOperand;
        } else {
            secondOperand = secondOperand * -1;
            return disp = secondOperand;
        }
    }

    const handlePercent = () => {
        if (disp === '0') { return };
        if (!selectedOperation) {
            firstOperand = firstOperand / 100;
            return disp = firstOperand;
        } else {
            secondOperand = secondOperand / 100;
            return disp = secondOperand;
        }
    }

    const handleClear = () => {
        disp = '0'
        firstOperand = '0';
        secondOperand = '0';
        selectedOperation = '';
    }
    
    const handleSelectedOperation = (operation: string) => {
        if (firstOperand === 0) { return };
        selectedOperation = operation;
    }

    const handleRes = () => {
        if (firstOperand === 0 || secondOperand === 0) { return };
        let res = '';
        switch (selectedOperation) {
            case '+':
                res = (parseFloat(firstOperand) + parseFloat(secondOperand)).toString();
                break;
            case '-':
                res = (parseFloat(firstOperand) - parseFloat(secondOperand)).toString();
                break;
            case 'x':
                res = (parseFloat(firstOperand) * parseFloat(secondOperand)).toString();
                break;
            case '/':
                res = ((parseFloat(firstOperand) / parseFloat(secondOperand)).toFixed(3)).toString();
                break;
            default:
                break;
        }
        disp = res;
        firstOperand = res;
        secondOperand = '0';
        selectedOperation = '';
    }
</script>

<main>
    <div>{firstOperand} {secondOperand}</div>
    <div class="relative flex mx-auto my-20 p-10 gap-y-5 flex-col justify-evenly items-center max-w-sm bg-black text-white text-3xl rounded-xl">
        <div class="absolute right-0 top-0 text-xl font-bold text-blue-500 mr-2">{selectedOperation}</div>
        <div class="bg-gray-900 text-right w-72 p-2 rounded-lg">{disp}</div>
        <div class="flex gap-x-4 divide-x">
            <div class="text-blue-500 grid grid-cols-3 gap-7">
                <button class="bg-gray-900 hover:bg-gray-800 active:bg-gray-700 rounded-lg" on:click={handleClear}>C</button>
                <button class="hover:bg-gray-900 active:bg-gray-700 rounded-lg" on:click={handlePlusMinus}>+/-</button>
                <button class="hover:bg-gray-900 active:bg-gray-700 rounded-lg" on:click={handlePercent}>%</button>
                {#each numbers as number (number)}
                    <button class="text-white hover:bg-gray-900 active:bg-gray-700 rounded-lg" on:click={() => handleNumClick(number)}>{number}</button>
                {/each}
                <button class="text-blue-500 bg-gray-900 hover:bg-gray-800 active:bg-gray-700 rounded-lg" on:click={handleRes}>=</button>
            </div>
            <div class="text-blue-500 grid gap-4 pl-4 text-2xl">
                <button class='text-blue-500 bg-gray-900 hover:bg-gray-800 active:bg-gray-700 rounded-lg px-2' on:click={handleDelete}>Del</button>
                {#each operations as operation (operation)}
                    <button class="hover:bg-gray-900 active:bg-gray-700 rounded-lg" on:click={() => handleSelectedOperation(operation)}> {operation} </button>
                {/each}
            </div>
        </div>
    </div>
</main>