
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