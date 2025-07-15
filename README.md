# Spatial ModernBERT

<p align="center">
    <img src="Assets/Spatial-ModernBERT.png" width="400"/>
</p>

<p align="center">
        📑 <a href="http://arxiv.org/abs/2507.08865"><b>Paper</b></a>&nbsp&nbsp | &nbsp&nbsp🤗 <a href="https://github.com/javis-admin/Spatial-ModernBERT">GitHub</a>&nbsp&nbsp | &nbsp&nbsp📊 <a href="#performance">Performance</a>&nbsp&nbsp
</p>

## Introduction

**Spatial ModernBERT** is a transformer-based model designed for accurate table and key-value extraction from complex financial documents. By integrating spatial embeddings with the ModernBERT architecture, our model excels at processing structured documents such as invoices, purchase orders, and receipts.

### Key Features

* **Multi-Headed Token Classification**: Simultaneous prediction of semantic labels, column indices, and row types through specialized classification heads
* **Advanced Spatial Embeddings**: Rich 768-dimensional spatial representations combining 2D positional coordinates and dimensional properties
* **B-I-IB Tagging Scheme**: Specialized tagging system for handling multi-line text fields common in financial documents
* **High Computational Efficiency**: Processes up to 60,000 tokens per second with near-linear scaling
* **Robust Performance**: Achieves 95.49% F1 score on CORD and 96.91% TEDS on PubTabNet

### Model Architecture

Our system extends ModernBERT-base with:
- **Spatial Embeddings**: 768-dimensional vectors encoding x_min, y_min, x_max, y_max, width, and height
- **Three Classification Heads**:
  - Label Head: Semantic classification (PO Number, Item Description, etc.)
  - Column Head: Column index prediction (0-9 with cycling)
  - Row Head: Row type classification (item rows vs header rows)
- **B-I-IB Tagging**: Handles multi-line fields with Beginning, Inside, and Inside-Below tags

## News

* **2024.07**: We have released the [Spatial ModernBERT Technical Report](http://arxiv.org/abs/2507.08865)
* **2024.07**: Initial release of Spatial ModernBERT for table and key-value extraction in financial documents

## Performance

### Benchmark Results

| Dataset | Our Model | Previous SOTA | Metric |
|---------|-----------|---------------|--------|
| CORD | **95.49** | 96.56 (LayoutLMv3) | F1 Score |
| FUNSD | 73.41 | 88.41 (LiLT) | F1 Score |
| FinTabNet | **98.09** | 98.21 (VAST) | TEDS |
| PubTabNet | **96.91** | 96.87 (MuTabNet) | TEDS |

### Inference Performance

| Batch Size | Sequence Length | Tokens Per Second |
|------------|----------------|-------------------|
| 1 | 512 | 29,402 |
| 8 | 512 | 59,409 |
| 64 | 512 | 60,985 |
| 8 | 1024 | 59,639 |
| 64 | 2048 | 58,527 |

## Model Architecture Details

### Spatial Embedding Computation

For each token with bounding box coordinates (x_min, y_min, x_max, y_max):

1. **Coordinate Embedding**: Each coordinate is embedded into 128-dimensional vectors
2. **Dimensional Features**: Width and height are computed and embedded separately
3. **Concatenation**: Six 128-dimensional vectors are concatenated to form 768-dimensional spatial embedding
4. **Fusion**: Spatial embeddings are added to text embeddings before transformer processing

## Datasets

### Training Data

- **PubTables-1M**: Pre-training on table structure recognition
- **Financial Document Dataset**: Fine-tuning on real-world financial documents with:
  - Invoice processing
  - Purchase order extraction
  - Receipt parsing
  - Key-value pair identification

### Evaluation Benchmarks

- **CORD**: Receipt understanding dataset
- **FUNSD**: Form understanding in noisy scanned documents
- **FinTabNet**: Financial table recognition
- **PubTabNet**: Scientific table recognition

## Applications

### Financial Document Processing

```python
# Example: Invoice processing
def process_invoice(image_path, model, processor):
    # OCR extraction
    ocr_results = extract_text_and_coords(image_path)
    
    # Model inference
    predictions = model.predict(ocr_results)
    
    # Post-processing
    structured_data = {
        'invoice_number': extract_field(predictions, 'invoice_number'),
        'date': extract_field(predictions, 'date'),
        'items': extract_table(predictions, 'items'),
        'total': extract_field(predictions, 'total')
    }
    
    return structured_data
```

### Integration with Business Workflows

- **Automated Invoice Processing**: Extract line items, totals, and metadata
- **Audit Trail Generation**: Maintain structured records for compliance
- **Data Analytics**: Convert documents to structured formats for analysis
- **ERP Integration**: Direct integration with enterprise systems

## Citation

If you find our paper and code useful in your research, please consider giving a star :star: and citation :pencil: :)

```BibTeX
@article{Spatial-ModernBERT,
  title={Spatial ModernBERT: Spatial-Aware Transformer for Table and Key-Value Extraction in Financial Documents at Scale},
  author={Amrendra Singh, Maulik Shah and Dharshan Sampath},
  journal={arXiv preprint arXiv:2507.08865},
  year={2025},
  url={http://arxiv.org/abs/2507.08865}
}
```

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Contributing

We welcome contributions! Please see our [Contributing Guidelines](CONTRIBUTING.md) for details.

## Contact

For questions and support:
- **Amrendra Singh**: amrendra.singh@javis.ai
- **Maulik Shah**: maulik.shah@javis.ai
- **GitHub Issues**: [Report issues](https://github.com/javis-admin/Spatial-ModernBERT/issues)

## Acknowledgments

We thank the ModernBERT team for providing the base transformer architecture and the research community for the valuable datasets and evaluation frameworks.