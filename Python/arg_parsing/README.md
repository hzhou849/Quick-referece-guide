# Argument parsing

* [Adding Descriptions](#adding-descriptions)
  - Help screen
  - epilogues
* [Flags](#flags)


## Adding Descriptions
### Help screen
Trigger help screens with ```-h or --help ``` flags.

### Epilogues allow you to add text after the argument definitions at the end(bottom) of screen

```python
def build_arg_parser() ->argparse.ArgumentParser:
    parser = argparse.ArgumentParser(
        description = "Generate  CSR and cache files", 
        epilog ="""
    Usage:
        Standard run:
            python %(prog)s 

        Use existing private key file:
            python %(prog)s --pkey .\\device-key.pem
        
        use custom config file:
            python %(prog)s --config .\\config.json
        
        Generate non-standard version=2 CSR:
            python %(prog)s --v2
    """,    
    formatter_class=argparse.RawDescriptionHelpFormatter # preserves line breaks in epilogue
    )
    
    parser.add_argument("--out-dir" , default="./output_files", help="Output directory")
    parser.add_argument( "--pkey", default = None, help="Path to existing EC Private key PEM(P256) file; If none, a new one is generated." )
    parser.add_argument("--key-pem", default="device-key.pem", help="Output private key PEM file")
    parser.add_argument("-o", "--csr-pem", default="device.csr.pem", help="Output CSR PEM format file")
    parser.add_argument("--cache-json", default="csr_cache.json", help="Output JSON cache filename")
    parser.add_argument("--skip-cache", action="store_true", help="Skip generating csr_cachce.json and checksum")
    
    # New arg for config path
    parser.add_argument("--config", default="config.json", help="Path to JSON config file to load in subject values")
    parser.add_argument("--v2", action="store_true", help="Generate non-standard CSR version=2")

```
