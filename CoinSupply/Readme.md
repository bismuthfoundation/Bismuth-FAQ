The following Python code calculates the total Bismuth coin supply when the block_height > 1200000.  
There was a hard fork which changed the mining rewards at block_height=1200000, and that explains why the total coin supply functions are written like this.  

Note also the SQL code after the comment symbol #: It calculates all mining rewards R0 from the genesis up to the hard fork at block_height=1200000.  

- The variable R1 contains the sum of all hypernode reward before block height 1200000.  
- The constant delta=2e-6 describes how much the mining rewards decrease for every new block, while the constant pos=2.4 describes the fixed reward for hypernodes.  
- The constant pow=10.2 describes the mining reward at block_height=1200000 and the variable N is the number of new blocks since the hardfork.  
- The constant dev_rew takes into account the 10% developer rewards which are calculated based on the mining rewards, and currently not on the hypernodes rewards.

~~~
def coin_supply(block):
    # select sum(reward) from transactions where block_height>0 and block_height<=1200000;
    R0 = 16572552.0048992
    # select sum(amount) from transactions where recipient='3e08b5538a4509d9daa99e01ca5912cda3e98a7f79ca01248c2bde16' and block_height>=-1200000;
    R1 = 320008
    delta=2e-6
    pos = 2.4
    pow = 10.2
    N = block - 1200000
    dev_rew = 1.1
    R = dev_rew*R0 + R1 + N*(pos + dev_rew*(pow - N/2*delta))
    return R;

x = 1222849
print("Coin Supply at block {} = {}".format(x,coin_supply(x)))
~~~

Another approximate function of the Bismuth coin supply based on timestamps instead of the Bismuth block_height is shown below:

~~~
from datetime import datetime

def coin_supply(t):
    R0 = 16572552.0048992
    R1 = 320008
    t0 = datetime(2019,6,6,11,32,9).timestamp() # Hardfork UTC time
    delta=2e-6
    pos = 2.4
    pow = 10.2
    N = (t - t0) / 60 # Estimated number of block since HF
    dev_rew = 1.1
    R = dev_rew*R0 + R1 + N*(pos + dev_rew*(pow - N/2*delta))
    return R;

x = datetime.now().timestamp() # Assuming system returns UTC time
print("Coin Supply at timestamp {} = {}".format(x,coin_supply(x)))
~~~
