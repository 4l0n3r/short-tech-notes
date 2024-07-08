Solidity:
    pragma
    import
    library, interface, errors
    contract
    type declarations
    state variables
    constant
    immutable
    events
    modifier
    function
    constructor
    receive
    fallback
    external
    public
    internal
    private
    view/pure

const wallet = new ethers.Wallet(process.env.PRIVATE_KEY)

const encryptedJsonKey = await wallet.encrypt(
process.env.PRIVATE_KEY
)

fs.writeFileSync("./.encryptedKey.json", encryptedJsonKey)

ethers:
    compile => abi, bin
    provider <== RPC_URL ( Node with testnet )
    wallet <== private_key + provider
    contractFactory <= wallet + bin + abi
    contract <= contractFactory.deploy()

hardhat:
    hardhat.config
    networks
    etherscan
    gasReporter

    CF <= etherContractFactory(ContractName)
    contract <= CF.deploy()
        contract.deployed()
        contract.deployTransaction.wait(6)

    verify:
        await run("verify:verify", { address: contractAddress, constructorArguments: args })

    yarn init
    yarn add --dev hardhat
    yarn hardhat
    yarn hardhat compile
    yarn hardhat run scripts/deploy.js --network sepolia


    scripts
    config file
    tests
    tasks
    local node
    verify contract using etherscan
    get gas reports using hardhat-gas-reporter
    solidity-coverage

    helper-hardhat-config
        chain specific default values
        developer networks

    deployments.fixture(["all"])

    chainlink:
        vrf
            create subscription
            add tokens to it
            use sub Id in our contract
            add consumer as our contract
        keepers
            register upkeep , add contract address there
            checkUpkeep -> on off chain, triggered automatically by chainlink keeper node and calculated boolean value
            performUpkeep -> on chain, will be triggered once the checkUpkeep returns true

        in development chain deploy the mock contract, else use the already deployed contract address

    ERC20
        many built in functions

    Defi
        aave , uniswap
        deposit collateral
        borrow another asset
        repay the asset

        forking
            create an app in alchemy to get the api url for forking

    troubleshoot
        -> yarn hardhat --verbose -> delete that file
        -> yarn install

Connecting with contract:
    ethers:
        typeof window.ethereum !== "undefined")
        ethereum.request({ method: "eth_requestAccounts" })

        provider => RPC_URL
        signer = provider.signer()
        contract = ethers.Contract(contractAddress, abi, signer)

    web3react:
        { active,activate,chainId,account,library: provider } = useWeb3React();
        injected = new InjectedConnector();
        activate(injected)

        same as above

    Web3Modal:
        providerOptions ( morethan metamask )
        web3Modal = new Web3Modal({ cacheProvider: false, providerOptions });
        web3ModalProvider = await web3Modal.connect();

        same as above ( just using the above provider as url )

    usedapp:
        activateBrowserWallet()
        contract = new ethers.Contract(contractAddress, abi);
        useContractFunction(contract,function_name,{args})

    moralis:
        { enableWeb3, isWeb3Enabled } = useMoralis()
        { data, error, runContractFunction, isFetching, isLoading } = useWeb3Contract(abi,address,function_name,params)

Optimizations:
    uint , uint256 no change until use inside struct
    use similar types in order
    always try to use view external ( view internal will also cost us )


default:
    yarn add --dev prettier prettier-plugin-solidity @nomiclabs/hardhat-etherscan dotenv
    yarn add --dev @nomiclabs/hardhat-ethers@npm:hardhat-deploy-ethers
    npm install --save-dev @nomiclabs/hardhat-ethers hardhat-deploy-ethers ethers

    yarn add --dev hardhat-gas-reporter prettier prettier-plugin-solidity solhint solidity-coverage dotenv
    yarn add --dev @nomiclabs/hardhat-ethers@npm:hardhat-deploy-ethers ethers @nomiclabs/hardhat-etherscan @nomiclabs/hardhat-waffle chai ethereum-waffle hardhat hardhat-deploy

    basic imports:
        yarn add --dev hardhat ethers@5.7.2 @nomiclabs/hardhat-ethers @nomicfoundation/hardhat-ethers@^3.0.2 @nomiclabs/hardhat-waffle ethereum-waffle @nomiclabs/hardhat-ethers@npm:hardhat-deploy-ethers hardhat-deploy @nomicfoundation/hardhat-verify
hardhat config:

Defi:
    eth is not possible to deposite since aave treats everything as ERC20 and eth is not under ERC20
    first deploy eth to get the weth ( wrapped eth )

    Create an API for forking the mainnet and interact with it locally.

References:

    https://www.reddit.com/r/solidity/comments/s6259q/what_are_some_good_beginner_projects/?rdt=34723
    
    Solidity CheatSheet : Overview
        https://topmonks.github.io/solidity_quick_ref/
        https://github.com/manojpramesh/solidity-cheatsheet?tab=readme-ov-file#using---for

    https://www.youtube.com/@smartcontractprogrammer/playlists

    NFT marketplace must try : https://blog.chain.link/how-to-build-an-nft-marketplace-with-hardhat-and-solidity/

NFT:
    npm install --save-dev @nomiclabs/hardhat-ethers
    openzeepline/contract
    Blog: https://blog.chain.link/how-to-build-an-nft-marketplace-with-hardhat-and-solidity/
    Code: https://github.com/PatrickAlphaC/hardhat-nft-marketplace-fcc
    Functionalities:
        AddItem
        UpdatePrice
        BoughtItem
        CancelItem
        WithdrawAmount


Table:
    31 hours videos
    scoffold eth    https://github.com/scaffold-eth/scaffold-eth-challenges/tree/challenge-0-simple-nft?tab=readme-ov-file
    speedrun ethereum
    build nft
    daap university https://go.dappuniversity.com/bootcamp-9245149b
    alchemy challenges https://docs.alchemy.com/docs/how-to-develop-an-nft-smart-contract-erc721-with-alchemy

Findings:
    can't return mapping.
    should add calldata, memory, storage to the struct variable
    make sure to set paths properly in hardhat config before executing any hardhat commands ( like deploy, test )
    return from transaction function is different from normal function call.
    should add await inside of the expect statement.
    if we don't import hardhat-deploy we should deploy the contract using "getContractFactory" function
    Always do the state changes first before calling any external function to prevent the Reentrancy attack
Troubleshoot:
    Removing toolbox from hardhat-config file
    Use toolbox instead of etherscan, waffle
    if msg.value -> then function should be payable
    toHexString()  -> Pass all args at least with the empty string
    runner is not allow to call -> create the deployer properly
    Edhaina output reflect akkatledhante "await" pettadam marchipoyavemo, check karlo
    TypeError: promise.then is not a function  =>  remove await from transaction call
    TypeError: Cannot read properties of undefined (reading 'length') =>  make sure you have "accounts" instead of "account"
    TypeError: Cannot read properties of undefined (reading 'JsonRpcProvider')  => change ethers version to 5.7.2
    TypeError: (0 , ethers_1.getAddress) is not a function => change ethers version to ^6.9.2 ( probably if we import hardhat-deploy )
    deployments is undefined => check the deploy dependencies and import hardhat-deploy to hardhat config file
    TypeError: (intermediate value) is not iterable   =>  remove the list and use curly braces on spread operator use.