~~~
public class VarArgsTest {
    public static final int LIMIT = 1_000_000;

    public static void main(String[] args) {

        VarArgsTest varArgsTest = new VarArgsTest();
        varArgsTest.something();

    }

    private void something() {
        final long start1 = System.nanoTime();
        for (int i = 0; i <= LIMIT; i++) {
            this.invokeOneParam(1,2,3,4,5);
        }
        System.out.println("invokeOneParam = " + ((System.nanoTime() - start1) / 1_000_000));

        final long start2 = System.nanoTime();
        for (int i = 0; i <= LIMIT; i++) {
            this.invokeVarArgs(1,2,3,4,5);
        }
        System.out.println("invokeVarArgs = " + ((System.nanoTime() - start2) / 1_000_000));


        final long start3 = System.nanoTime();
        IntStream.rangeClosed(0, LIMIT)
                .forEach(i -> invokeOneParam(1,2,3,4,5));
        System.out.println("invokeOneParam = " + ((System.nanoTime() - start3) / 1_000_000));

        final long start4 = System.nanoTime();
        IntStream.rangeClosed(0, LIMIT)
                .forEach(i -> invokeVarArgs(1,2,3,4,5));
        System.out.println("invokeVarArgs = " + ((System.nanoTime() - start4) / 1_000_000));
    }

    private void invokeOneParam(int a, int b, int c, int d, int e) {
    }

    private void invokeVarArgs(int ... ints) {
    }

}
~~~
